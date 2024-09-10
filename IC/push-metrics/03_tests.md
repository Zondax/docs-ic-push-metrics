## ic-os

Once all the changes has been applied in our fork repo, the build process for hostos and guestos has been done following the bazel procedure explained, [managing-ic-os-files](https://github.com/dfinity/ic/tree/master/ic-os#managing-ic-os-files)

### build

Due to the docker image with vector installed has not been published yet, the ic nodes images has been generated using the bazel environment `local-base-dev`.

```
bazel build //ic-os/{hostos,guestos}/envs/local-base-dev
```

### run

- hostos:
```
cd bazel-out/k8-opt/bin/ic-os/hostos/envs/local-base-dev
tar xvf disk.img.tar
qemu-system-x86_64 -machine type=q35 -nographic -m 4G -bios /usr/share/OVMF/OVMF_CODE.fd -drive file=disk.img,format=raw,if=virtio -netdev user,id=user.0,hostfwd=tcp::2222-:22 -device virtio-net,netdev=user.0
```

- guestos:
```
cd bazel-out/k8-opt/bin/ic-os/guestos/envs/local-base-dev
tar xvf disk.img.tar
qemu-system-x86_64 -machine type=q35 -nographic -m 4G -bios /usr/share/OVMF/OVMF_CODE.fd -drive file=disk.img,format=raw,if=virtio -netdev user,id=user.0,hostfwd=tcp::2222-:22 -device virtio-net,netdev=user.0
```

## Tests

A test enviroment based on containers has been built to run elasticsearch, victoriametrics and a reverse proxy to manage mTLS auth from the simulated nodes. Simulated nodes, hostos and guestos, has been deployed based on vms, running them with qemu. Due to the docker image with vector installed has not been published yet, the ic nodes images has been generated using the bazel environment `local-base-dev`.

![Test architecture](./assets/IC_metrics_proposed_architecture_mtls.jpg)

### mtls

The mTLS configuration has been included to check the concept during the tests. This mTLS configuration requires manual steps and is not intended to be used as production deployment.

These are the files and scripts that has been used to generate the CA and the server and client certificates:
* cert.config
```
[ req ]
distinguished_name     = req_distinguished_name
x509_extensions        = v3_ca

dirstring_type = nobmp

[ req_distinguished_name ]
countryName                    = Country Name (2 letter code)
countryName_value              = CH

localityName                   = Locality Name (eg, city)
localityName_value             = Zug

organizationName               = Organization Name (e.g., company)
organizationName_value         = Zondax

organizationalUnitName         = Organizational Unit Name (eg, section)
organizationalUnitName_value   = mtls

commonName                     = Common Name (eg, YOUR name)
#commonName_value               = mtls.zondax.ch

emailAddress                   = Email Address
emailAddress_value             = mtls@zondax.ch

[ v3_ca ]

basicConstraints               = critical,CA:TRUE, pathlen:3
keyUsage                       = critical, keyCertSign, digitalSignature, cRLSign, nonRepudiation
```
* generate_CA.sh
```
#!/usr/bin/env bash

ALGORITHM=ed25519
CA_NAME=ca
CA_DURATION=3652
CERT_CONFIG=cert.config
CERT_DURATION=365
TLS_PATH=tls
SERVER_NAME=mtls-server
CLIENT_NAME=mtls-client01

if [ ! -f ${TLS_PATH}/${CA_NAME}.key ]; then
    openssl genpkey -algorithm ${ALGORITHM} > ${TLS_PATH}/${CA_NAME}.key
    chmod 400 ${TLS_PATH}/${CA_NAME}.key
    openssl pkey -in ${TLS_PATH}/${CA_NAME}.key -pubout -out ${TLS_PATH}/${CA_NAME}.pub
    openssl req -config ${CERT_CONFIG} -new -x509 -days ${CA_DURATION} -key ${TLS_PATH}/${CA_NAME}.key -out ${TLS_PATH}/${CA_NAME}.crt
fi

if [ ! -f ${TLS_PATH}/${SERVER_NAME}.key ]; then
    openssl genpkey -algorithm ${ALGORITHM} > ${TLS_PATH}/${SERVER_NAME}.key
    chmod 400 ${TLS_PATH}/${SERVER_NAME}.key
    openssl pkey -in ${TLS_PATH}/${SERVER_NAME}.key -pubout -out ${TLS_PATH}/${SERVER_NAME}.pub
    openssl req -config ${CERT_CONFIG} -new -x509 -days ${CERT_DURATION} -CA ${TLS_PATH}/${CA_NAME}.crt -CAkey ${TLS_PATH}/${CA_NAME}.key -key ${TLS_PATH}/${SERVER_NAME}.key -out ${TLS_PATH}/${SERVER_NAME}.crt
fi

if [ ! -f ${TLS_PATH}/${CLIENT_NAME}.key ]; then
    openssl genpkey -algorithm ${ALGORITHM} > ${TLS_PATH}/${CLIENT_NAME}.key
    chmod 400 ${TLS_PATH}/${CLIENT_NAME}.key
    openssl pkey -in ${TLS_PATH}/${CLIENT_NAME}.key -pubout -out ${TLS_PATH}/${CLIENT_NAME}.pub
    openssl req -config ${CERT_CONFIG} -new -x509 -days ${CERT_DURATION} -CA ${TLS_PATH}/${CA_NAME}.crt -CAkey ${TLS_PATH}/${CA_NAME}.key -key ${TLS_PATH}/${CLIENT_NAME}.key -out ${TLS_PATH}/${CLIENT_NAME}.crt
fi
```

* mtls-nginx.conf
```
user nginx;
worker_processes 2;

error_log /var/log/nginx/error.log notice;
pid /var/run/nginx.pid;


events {
    worker_connections 1024;
}


http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;

    charset utf-8;
    server_tokens off;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    client_max_body_size 16M;

    #gzip on;

    server {
        listen 80 default_server;
        return 301 https://$host$request_uri;
    }
    server {
        listen 443 ssl;

        # Server certificate and key
        ssl_certificate /tmp/tls/mtls-server.crt;
        ssl_certificate_key /tmp//tls/mtls-server.key;

        # CA certificate for client verification
        ssl_client_certificate /tmp/tls/ca.crt;
        ssl_verify_client on;

        location /logs/ {
            proxy_pass http://elasticsearch:9200/;

            proxy_buffering off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        location /metrics/ {
            proxy_pass http://victoriametrics:8428/;

            proxy_buffering off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

### compose

Compose file to deploy test environment

```
services:
  victoriametrics:
    container_name: victoriametrics
    image: docker.io/victoriametrics/victoria-metrics:v1.102.1
    ports:
      - 8428:8428
    volumes:
      - vmdata:/storage
    command:
      - "--storageDataPath=/storage"
      - "--httpListenAddr=:8428"
    networks:
      - icp_net
    restart: always
  elasticsearch:
    container_name: elasticsearch
    image: docker.elastic.co/elasticsearch/elasticsearch:8.15.0
    ports:
      - 9200:9200
    mem_limit: 1GB
    environment: ['bootstrap.memory_lock=true','discovery.type=single-node','xpack.security.enabled=false', 'xpack.security.enrollment.enabled=false']
    networks:
      - icp_net
    restart: always
  mtls-server:
    container_name: mtls-server
    image: docker.io/nginx:1.27.1-alpine3.20-slim
    ports:
      - 8080:80
      - 8443:443
    volumes:
      - ./mtls/tls:/tmp/tls:ro
      - ./mtls/mtls-nginx.conf:/etc/nginx/nginx.conf:ro
    networks:
      - icp_net
    restart: always
volumes:
  vmdata: {}
networks:
  icp_net:
```
