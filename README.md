# Intro

This is a working draft of possible topics to be considered in order to secure and standardize the company's most important resources for the new project.

The initial objectives are:
- List the resources that have an important relevance for the platform.
- Search for relevant topics to be considered as good practices related to SOC2 or similar.
- Search for tools that are implemented or can be implemented in order to comply with these topics.
- See the status of currently implemented solutions.
- Define an adequate periodicity to review these topics by the Platform team.
- Define possible TODOs to be performed as tasks and prioritize them.

*NOTE*: SOC2 does not specify technologies or tools in particular, but focuses on the oversight of controls and processes to ensure security, availability, information processing, confidentiality and privacy integrity.

## Index
- [1. Source Control](#1-source-control)
    - [GitHub](#github)
    - [Source Code](#source-code)
- [2. Containers and Charts](#2-containers-and-charts)
    - [Container Images](#container-images)
    - [Helm Charts](#helm-charts)
- [3. CI/CD](#3-cicd)
    - [GitHub Actions (CI)](#github-actions-ci)
    - [ArgoCD and/or Flux.CD (CD)](#argocd-andor-fluxcd-cd)
- [4. Infrastructure](#4-infrastructure)
    - [Proxmox](#proxmox)
    - [Ceph](#ceph)
    - [CloudFlare](#cloudflare)
    - [Terraform](#terraform)
    - [Servers](#servers)
    - [Kubernetes Clusters](#kubernetes-clusters)
    - [General Monitoring and Logs](#general-monitoring-and-logs)
- [5. Security](#5-security)
    - [Firewalls](#firewalls)
    - [Credentials](#credentials)
    - [Backups](#backups)
    - [PKI (Public Key Infrastructure)](#pki-public-key-infrastructure)


## 1. Source Control

### GitHub

#### Relevant topics to consider

- **Access Control**: Restrict access to repositories and ensure that only authorized persons can make changes.
    - [GitHub](https://docs.github.com/en/code-security/getting-started/securing-your-organization)
    - [GitGuardian](https://www.gitguardian.com)

- **Audit and Tracking**: Maintain records of all code modifications to identify any unusual activity.
    - [GitHub](https://docs.github.com/es/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization)
    - [GitHub Audit Log API](https://docs.github.com/en/rest/reference/audit-log)

#### Initial periodicity

TBD

#### Current status

TBD

#### Possible TODOs
- Workflow for external customers (2x PRs with branch, credentials at Repository level...)
- Administration and permissions on repositories in their creation (disabling the wiki, forums, etc...).

### Source Code

#### Relevant topics to consider

- **Access Control**: Restrict access to source code to authorized personnel.
- **Change Management**: Formal processes for code reviews and approvals prior to merges.
- **Auditing**: Maintaining detailed records of who changed what and when in the source code.
- **Code Security**: Systematic analysis of security vulnerabilities or flaws in the code and dependencies.
    - [SonarQube](https://www.sonarsource.com/products/sonarqube/)
- **Code Integrity**: Ensuring that code has not been modified in an unauthorized manner.
- **Backup and Recovery**: Effective policies for backing up and recovering source code.

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
- Check the status of repositories using CodeQL, DeepSource or Sentry tools.
- Check if the tools meet the requirements they should have for the relevant topics


## 2. Containers and Charts

### Container Images

#### Relevant topics to consider
- **Container Level Security**: Apply security practices such as vulnerability scanning and avoid running containers with elevated privileges.
    - [Clair](https://github.com/quay/clair)
    - [Trivy](https://trivy.dev/)
    - Harbor
    - [Anchore Engine](https://github.com/anchore/anchore-engine)
    - [Falco](https://falco.org/)
    - [Kyverno](https://neonmirrors.net/post/2022-07/attesting-image-scans-kyverno/)

#### Initial periodicity
1x Week

#### Current status
TBD

#### Possible TODOs
- Check Kyverno's support for this type of task and see if it complies with the relevant topics.
- Study the deployment of vulnerability scanning applications for registered containers.
- Check the possibility of replacing DocherHub with another alternative

### Helm Charts

#### Relevant topics to consider
- **Template Verification**: Audit that Helm templates are secure and free of configurations that may introduce risks.
    - [Helm Lint](https://github.com/helm/helm/tree/master/pkg/lint)

#### Initial periodicity
TBD

#### Current status
Done into the CI

#### Possible TODOs
TBD



## 3. CI/CD

### GitHub Actions (CI)

#### Relevant topics to consider
- **Review of Actions**: Check that the actions used in the workflow are from reliable sources and are well maintained.
- **GitHub Secrets**: Use GitHub's secrets functionality to store sensitive environment variables.
- **Rotate Secrets**: Automate the rotation of secrets periodically to reduce security risks.
- **GitHub Actions Logs**: Ensure that workflow execution logs are detailed, secure, and available for auditing.
- **Log Access Control**: Limit access to GitHub Actions logs to authorized users.
- **Branch Protection**: Configure protected branches to require code reviews and prevent direct changes to critical branches.
- **Automated Testing and Code Checks**: Integrate automated testing and code quality or style checks before allowing pull requests to be merged.
- **Conditional Steps**: Use conditional steps to execute certain tasks only if certain security or quality conditions are met.
- **Workflows as Code**: Maintain versioned and reviewed workflows as part of the source code for traceability and compliance.

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
TBD


### ArgoCD and/or Flux.CD (CD)

#### Relevant topics to consider
- **Automation and Orchestration**: Have clear and auditable processes for automated deployments, including code reviews and approvals.
- **Monitoring**: Monitoring of synchronized application status and possible errors.
    - Monitoring with Prometheus and Grafana

#### Initial periodicity
Daily

#### Current status
TBD

#### Possible TODOs
TBD



## 4. Infrastructure

### Proxmox

We have been proactive about this. We must consider the lifecycle (6 months) and checking the new releases

#### Relevant topics to consider

- **Resource Isolation**: Verify that each virtualized environment is secure and does not compromise other environments.

- **Backup and Recovery**: Ensure that adequate backup and disaster recovery procedures are in place.

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
- Be aware of the lifecycle and actively check (using a calendar or similar task) for new releases and what they affect, in addition to quickfix releases.

### Ceph

#### Relevant topics to consider
- **Data Protection**: Ensure that stored data is encrypted and that there is adequate data segregation.
    - Monitoring with Prometheus and Grafana

#### Initial periodicity
Daily

#### Current status
TBD

#### Possible TODOs
- See the encryption options (may be problematic for performance) that may be implemented in detail.
- See in detail the space reservation/capacity by applications to check that space reservations are not being overdimensioned.
- Check status in Dashboard Grafana.


### CloudFlare

#### Relevant topics to consider
- **Network Protection**: Ensure that CloudFlare's CDN and DDoS functionalities are properly configured to protect against external attacks.
- **Event Logging**: Monitor and log all traffic passing through CloudFlare to detect potential security breaches.

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
- Check enabled CloudFlare resources at WAF and DoS protection level.
- Check access to CloudFlare Logs and the possibility of integrating them with an external Dashboard (or use CloudFlare's own).

### Terraform 

CLI, providers and Zondax modules are considered.

#### Relevant topics to consider
- **Infrastructure Code**: Review and monitor Terraform scripts to prevent configurations that could expose resources to unnecessary risk.
    - [Checkov](https://www.checkov.io)
    - [Terrascan](https://github.com/accurics/terrascan)

#### Initial periodicity
3 months for Terraform providers

#### Current status
TBD

#### Possible TODOs
- Update web42 documentation to reflect the need to use the version currently used by Atlantis (1.5.7).
- See the feasibility of using OpenTOFU (1.6) as a replacement for Terraform for the following releases
- Use terraform configuration scanning tools in the CI/CD pipeline (Flux/Atlantis). 
- Periodically check the status of the latest versions and providers' changelogs
- Check the traceability of changes in Zondax modules using Releases (grouping the PRs) to make the change view easier.

### Servers

#### Relevant topics to consider
- **Access Control**: Restrict access to resources according to the principle of least privilege.
- **Authentication and Authorization**: Use of strong passwords, SSH keys, and possibly multifactor authentication.
- **Encryption**: Protect data in transit and at rest with appropriate encryption techniques.
- **Patch Management**: Regularly apply security updates and patches.
- **System Monitoring**: Implement monitoring tools to alert on conditions that could lead to unavailability.
- **Backups**: Ensure regular backups and test restores periodically.
- **Logging and Auditing**: Log and audit critical actions and system changes.
- **Data Integrity**: Verify the integrity of stored and processed data.
- **Sensitive Data Management**: Protect the confidentiality of sensitive information through access controls and encryption.
- **Privacy Policy**: Define clear policies for handling private and sensitive data.
- **Configuration Management**: Maintain standardized and controlled system configurations.
- **Training and Awareness**: Train staff in security and privacy practices.

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
- Perform security audits of servers at the service, operating system and package levels.
- Study Ubuntu LTS Enterprise solutions for hot upgrades including kernel with kexec.
- TBD


### Kubernetes Clusters

#### Relevant topics to consider
- **Container Security**: Implement security policies to control access and manage vulnerabilities in containers.

- **Configuration Management**: Ensure Kubernetes configurations follow best practices to maintain integrity and security.
    - [Kubeaudit](https://github.com/Shopify/kubeaudit)
    - [Kyverno]()
    - [Polaris](https://github.com/FairwindsOps/polaris)

- **Monitoring**: Need to properly monitor clusters to ensure security, availability, information processing, confidentiality and privacy integrity.


#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
- Check new releases and Changelogs of rke2/k3s for upgrade cycles if needed.
- Periodically check cluster status (at Node NotReady level, CrashLoopBack Pods, Evicted, Restarts...)

### General Monitoring and Logs

#### Relevant topics to consider
- **Centralized Logging and Monitoring**: 
    - [ELK Stack](https://www.elastic.co/what-is/elk-stack) (Elasticsearch, Logstash, Kibana) 
    - [Graylog](https://www.graylog.org/)
- **File Integrity Monitoring**: 
    - [Wazuh](https://wazuh.com/)
    - [AIDE](http://aide.sourceforge.net/)

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
TBD

## 5. Security

## Firewalls

#### Relevant topics to consider

- **Monitoring and Rules Management**: Keep firewall rules up to date and review them periodically to protect against emerging threats.
- **Release version**: Firewall software version and rule updates.

#### Initial periodicity
Daily for monitoring
TBD for software releases

#### Current status
TBD

#### Possible TODOs
- Checking firewall versions and rules
- Check the possibility of a dashboard to see the existing attack and blocking logs.
- Use PagerDuty for monitoring security incidents. 
security.
- Study the problem of segmentation between environments due to the current network configuration.
- Study the possibility of using an IDS in Opensense after the Firewall layer.

---


## Credentials

#### Relevant topics to consider
- **Secrets Management**: Use tools like HashiCorp Vault or 1Password for secure management of passwords and secrets, and apply least privilege principles.
- **Audit logs**: Ensuring traceability and integrity of access and changes in the management of secrets
    - [1Password](https://developer.1password.com/docs/events-api/audit-events/)
    - [HashiCorp Vault - Vault Audit Logging](https://github.com/AlyRagab/Vault-Audit-Logging)
    - [Vault + Boundary](https://japneetsahni.com/blog/zero-trust-security-using-hashicorp-boundary-vault/)

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
- Check access to 1password audit logs and Vault logs. In the case of Vault it seems that access is more complicated than in 1password.

## Backups

#### Relevant topics to consider
- **Backup Policies**: Establish regular and secure backup policies and test the restore process.

#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
TBD

## PKI (Public Key Infrastructure)

#### Relevant topics to consider
- **Certificate Management**: Properly manage digital certificates for authentication and encryption, along with their renewal and revocation when necessary.
    - [step-ca](https://github.com/smallstep/certificates)
    - [Letsencrypt]()
#### Initial periodicity
TBD

#### Current status
TBD

#### Possible TODOs
TBD