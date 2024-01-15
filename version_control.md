# Version Control

## Index

- [Kubernetes RKE2](#kubernetes-rke2)
- [Canal](#canal)
- [Cilium](#cilium)
- [Ingress](#ingress)
- [ArgoCD](#argocd)
- [OnePassword](#onepassword)
- [Prometheus CRDs](#prometheus-crds)
- [Prometheus and AlertManager](#prometheus-and-alertmanager)
- [Loki](#loki)
- [Grafana](#grafana)
- [Atlantis](#atlantis)
- [DEX](#dex)
- [Cert Manager](#cert-manager)
- [External DNS](#external-dns)
- [Cloudflared](#cloudflared)

## Kubernetes RKE2
### Deployment and Setup
- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf
        cluster = {
            [...]
            version      = "v1.26.7+rke2r1"
        }
```
- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
    node_install_k8s.sh.tmpl
        https://get.$K8S_KIND.io
        install_k8s function
```
### Releases
- [GitHub Atom RSS Feed](https://github.com/rancher/rke2/releases.atom)
- Release Cycle: "We produce RKE2 releases for every upstream Kubernetes release, and follow the same end-of-life schedule: https://kubernetes.io/releases/"


## Canal
### Deployment and Setup
- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf
        cni = {
            name = "canal"
        }
```
- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
    locals.tf
        cluster_cni_template = {
            [...]
        }
        cni_defaults = {
            [...]
            "${local.cni_canal}" = {
                version = "v3.26.3-build20231109"
                chart   = "rke2-canal"
                repo    = "https://github.com/rancher/rke2-charts"
                [...]
            }
        }
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/rke2-charts/rke2-canal/feed/rss) - Same as RKE2
- Release Cycle: "We produce RKE2 releases for every upstream Kubernetes release, and follow the same end-of-life schedule: https://kubernetes.io/releases/"


## Cilium
### Deployment and Setup
- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf
        cni = {
            name = "cilium"
            version = "v14.4.4"
            [...]
        }
```
- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf

    cluster_cni_template = {
        [...]
    }
    cni_defaults = {
        [...]
        "${local.cni_cilium}" = {
            version = "v1.14.4"
            chart   = "cilium"
            repo    = "https://helm.cilium.io/"
            [...]
        }
    }
```
### Releases
- [GitHub Atom RSS Feed](https://github.com/cilium/cilium/releases.atom)
- [Release Cycle](https://www.versio.io/product-release-end-of-life-eol-Cilium-Cilium.html)



## Ingress
### Deployment and Setup

- https://github.com/Zondax/zondax-tf-k8s-cluster/
    
```    
    locals.tf 

        cluster_ingress = local.cluster_kind == local.cluster_rke2 ? contains(local.cluster_disable.addons, "rke2-ingress-nginx") && local.cluster_cni.name == local.cni_cilium ? local.cni_cilium : "nginx" : "traefik"
        [...]
        cluster_apps_default = {
            registry = {
                content = templatefile("${path.module}/files/apps/registry-manifest.tmpl",
                    {
                        [...]
                        ingress_class      = local.cluster_opt_apps.registry ? local.cluster_ingress : ""
                    }
            }
        }
        argocd = {
            content = templatefile("${path.module}/files/apps/argocd-manifests.tmpl",
                {
                    [...]
                    ingress    = local.cluster_ingress
                    [...]
                })
        }
```
### Releases
- [GitHub Atom RSS Feed](https://github.com/rancher/rke2/releases.atom) - Same as RKE2
- Release Cycle: "We produce RKE2 releases for every upstream Kubernetes release, and follow the same end-of-life schedule: https://kubernetes.io/releases/"


## ArgoCD
### Deployment and Setup
- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf

        extra_apps = ["argocd", "..."]
        opt_items = {
            argocd = {
                version              = "5.51.1"
                app_version          = "v2.9.0"
                plugin_sidecar_image = "quay.io/argoproj/argocd:v2.9.0"
                vault_plugin_version = "1.17.0"
            }
        }
```
- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
    locals.tf 
        cluster_apps_default = {
            [...]
            argocd = {
                content = templatefile("${path.module}/files/apps/argocd-manifests.tmpl",
                    {
                        version = local.cluster_opt_apps.argocd ? var.opt_items.argocd["version"] : ""
                        app_version = local.cluster_opt_apps.argocd ? var.opt_items.argocd["app_version"] : ""
                        plugin_sidecar_image = local.cluster_opt_apps.argocd ? var.opt_items.argocd["plugin_sidecar_image"] : ""
                        vault_plugin_version = local.cluster_opt_apps.argocd ? var.opt_items.argocd["vault_plugin_version"] : ""
                        [...]
                    }
            }
    }

    files/apps/argocd-manifests.tmpl
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/argo/argo-cd/feed/rss)
- [Release Cycle](https://argo-cd.readthedocs.io/en/stable/developer-guide/release-process-and-cadence/) 


## OnePassword
### Deployment and Setup

- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf
        extra_apps = ["...", "onepassword", "..."]
        opt_items = {
            [...]
            onepassword = {
                app_version          = "1.7.2"
                version              = "1.14.0"
            }
            [...]
        }
```

- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
    locals.tf 
        cluster_apps_default = {
            [...]
            onepassword = {
                content = templatefile("${path.module}/files/apps/onepassword-manifest.tmpl",
                    {
                        app_version = local.cluster_opt_apps.onepassword ? var.opt_items.onepassword["app_version"] : "1.7.2"
                        version          = local.cluster_opt_apps.onepassword ? var.opt_items.onepassword["version"] : "1.14.0"
                    }
            })
        }

    files/apps/onepassword-manifest.tmpl
```
### Releases
- [GitHub Atom RSS Feed](https://github.com/1Password/connect-helm-charts/releases.atom)
- Release Cycle: To be calculated (not available)


## Prometheus CRDs
### Deployment and Setup
- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf
        extra_apps = ["...", "prometheus-crds"]
        opt_items = {
            [...]
            prometheus_crds = {
                version     = "6.0.0"
                app_version = "0.68.0"
            }
            [...]
        }
```

- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
    locals.tf
        cluster_apps_default = {
                [...]
            prometheus-crds = { 
                content = templatefile("${path.module}/files/apps/prometheus-crds-manifest.tmpl",
                    {   
                        version = local.cluster_opt_apps.prometheus_crds ? var.opt_items.prometheus_crds["version"] : ""
                    }
            })
        }

    files/apps/prometheus-crds-manifest.tmpl
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/prometheus-community/prometheus-operator-crds/feed/rss)
- [Release Cycle](https://prometheus.io/docs/introduction/release-cycle/) 


## Prometheus and AlertManager
### Deployment and Setup
- https://github.com/Zondax/project-zondax/
```
    ${ENVIRONMENT}/apps/prometheus
        config.yaml
            app:
                namespace: prometheus
                source: oci://registry.zondax.dev/zondax/prometheus
                version: 46.2.0

        values.yaml
```
- https://github.com/Zondax/helm-charts/
```
    charts/prometheus
        Chart.yaml
            [...]
            dependencies:
            - name: kube-prometheus-stack
            repository: https://prometheus-community.github.io/helm-charts
            # If the appVersion is changed - the prometheus-crds must be updated FIRST!
            version: 51.2.0
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/prometheus-community/kube-prometheus-stack/feed/rss)
- [Release Cycle](https://prometheus.io/docs/introduction/release-cycle/)

## Loki
### Deployment and Setup
- https://github.com/Zondax/project-zondax/
```
    ${ENVIRONMENT}/apps/loki-stack
        config.yaml
            app:
                namespace: logging
                source: oci://registry.zondax.ch/zondax/loki-stack
                version: 2.9.11-0

        values.yaml
```
- https://github.com/Zondax/helm-charts/
```
    charts/prometheus
        Chart.yaml
                [...]
                dependencies:
                - name: loki-stack
                repository: https://grafana.github.io/helm-charts
                version: 2.9.11
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/grafana/loki/feed/rss)
- [Release Cycle](https://grafana.com/docs/release-life-cycle/) - Same as Grafana

## Grafana
### Deployment and Setup
- https://github.com/Zondax/project-zondax-ups/
```
    ${ENVIRONMENT}/apps/grafana
        config.yaml
            app:
                namespace: grafana
                source: oci://registry.zondax.xyz/zondax/grafana
                version: 0.4.2
```
- https://github.com/Zondax/helm-charts/
```
    charts/grafana
        Chart.yaml
            [...]
            dependencies:
                - name: grafana
                repository: https://grafana.github.io/helm-charts
                version: 6.58.1
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/grafana/grafana/feed/rss)
- [Release Cycle](https://grafana.com/docs/release-life-cycle/) - Same as Grafana

## Atlantis
### Deployment and Setup

Previous deployment-style (project- + app-).
Has dependencies with 1Password connect helm chart.

- https://github.com/Zondax/zondax-projects
```
    ${ENVIRONMENT}/values.yaml
```
- https://github.com/Zondax/project-atlantis
```
    ${ENVIRONMENT}/apps/atlantis
        config.yaml
        app:
            namespace: server
            kind: helm             
            extraArgs: ""
        repo:
            url: https://github.com/Zondax/app-atlantis.git
            revision: master
            path: adm
```

- https://github.com/Zondax/app-atlantis
```
    ${ENVIRONMENT}
        dependencies:
        - name: atlantis
        repository: https://runatlantis.github.io/helm-charts
        version: 4.15.1
        - name: connect
        repository: https://1password.github.io/connect-helm-charts/
        version: 1.14.0
        alias: connect${ENVIRONMENT}
```
### Releases
- [GitHub Atom RSS Feed](https://github.com/runatlantis/helm-charts/releases.atom)
- Release Cycle: To be calculated (not available)

## DEX
### Deployment and Setup
- https://github.com/Zondax/project-zondax-ups/
```
    ${ENVIRONMENT}/apps/dex
        config.yaml
            app:
                namespace: dex
                source: oci://registry.zondax.xyz/zondax/dex
                version: 1.0.5
```
- https://github.com/Zondax/helm-charts
```
    charts/dex
        Chart.yaml
            [...]
            dependencies:
            - name: dex
            repository: https://charts.dexidp.io
            version: 0.15.3
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/dex/dex/feed/rss)
- Release Cycle: To be calculated (not available)

## Cert Manager
### Deployment and Setup
- https://github.com/Zondax/deploy-k8s-clusters
```
        ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf
            [...]
            cert_manager_extra = {
                [...]
                version     = "v1.12.4"
                app_version = "v1.12.4"
                [...]
            }
```
- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
        locals.tf
            cert_manager_data = {
                [...]
                version     = lookup(var.cert_manager_extra, "version", "v1.12.4")
                app_version = lookup(var.cert_manager_extra, "app_version", "v1.12.4")
            }

            cluster_apps_default = {
                    [...]
                    cert-manager = {
                        content = templatefile("${path.module}/files/apps/cert-manager-manifest.tmpl",
                            {
                                [...]
                                version = local.cert_manager_data.version
                                app_version = local.cert_manager_data.app_version
                                [...]
                            }
        
        files/apps/cert-manager-manifest.tmpl
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/dex/dex/feed/rss)
- [Release Cycle](https://cert-manager.io/docs/releases/)

## External DNS
### Deployment and Setup

- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/${CLUSTER_NAME}-${CLUSTER_KIND}.tf
        [...]
        external_dns_extra = {
            [...]
            version     = "6.24.1"
            app_version = "0.13.5"
            [...]
        }
```
- https://github.com/Zondax/zondax-tf-k8s-cluster/
```
    locals.tf
        [...]
        cluster_apps_default = {
            external-dns = { 
                content = templatefile("${path.module}/files/apps/external-dns-manifest.tmpl",
                    {
                        [...]
                        version = lookup(var.external_dns_extra, "version", "6.24.1")
                        app_version   = lookup(var.external_dns_extra, "app_version", "0.13.5")
                        [...]
                    })
            }

    files/apps/external-dns-manifest.tmpl
```
### Releases
- [ArtifactHub RSS Feed](https://artifacthub.io/api/v1/packages/helm/bitnami/external-dns/feed/rss)
- [Release Cycle](https://kubernetes-sigs.github.io/external-dns/v0.12.0/release/)

## Cloudflared
### Deployment and Setup
- https://github.com/Zondax/deploy-k8s-clusters
```
    ${ENVIRONMENT}/${CLUSTER_NAME}/tunnels.tf
        [...]
        cloudflared_version = "2023.8.2"
        [...]
```

- https://github.com/Zondax/zondax-tf-k8s-cloudflared
```
    cloudflared.tf
        [...]
            tunnels_deploy = { for tunnel_key, tunnel in local.tunnels :
                tunnel_key => {
                    content = templatefile("${path.module}/files/cloudflare-deployment.yaml.tmpl",
                    [...]
                    version = var.cloudflared_version
                    [...]
                }
            }

    files/cloudflare-deployment.yaml.tmpl
```
### Releases
- [GitHub Atom RSS Feed](https://github.com/cloudflare/cloudflared/releases.atom)
- [Release Cycle](https://github.com/cloudflare/cloudflared/blob/master/RELEASE_NOTES)
