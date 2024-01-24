# Part I - Introduction

## 1. Introduction (5 minutes)
- Introduce yourself
- Welcoming environment
- Brief description of the interview process and participants
    - Zondax
    - Carlos
    - Raul
    - Technical interview, coffee-talk style
    - Next steps interview with the CEO, Juan

## 2. Candidate presentation (5 minutes)
- Experience
- Key skills
- Motivation for the SRE position and working with Zondax

# Part II. Technical

## 1. Scalability and deployments (IaC and k8s) (10 minutes)
- Experiences in the design and management of scalable systems.
- Implementation of deployments on IaC / K8S
Example IaC: Infrastructure as Code with Terraform
Create a Terraform script to provision a Kubernetes cluster on a cloud provider (can be on-premises k3s or MiniKube or any other Cloud provider with a free tier such as GCP, AWS, Azure). Ensure that the Terraform script follows best practices in terms of files, variables, locals and functionality.
Example Helm Chart/k8s: Application Deployment with Helm and Kubernetes
Create a Helm Chart for a simple web application (it can be an Nginx web server with a static welcome page or similar). Include in the Chart different configurations for separate deployments per environment (e.g. development, pre-production and production). Deploy the application in the cluster created with Helm with the simulated production environment.

## 2. Monitoring, alerts and troubleshooting (10 minutes)
- Previous experience with the use of monitoring systems
    grafana, prometheus, elastic, loki
- Ability to manage alerts
    alertmanager, pagerduty
- Ability to address and resolve operational issues
    tools for using (k9s, lens, kubectl)
Example: Set up basic alerts and monitoring for the application using Prometheus and Grafana. Custom alerts.

## 3. Automation and tools (10 minutes)
Experience in automation and use of specific tools
    - ArgoCD
    - FluxCD
Focus on improving corporate efficiency
    - Development for automations
        - Go, Python, Rust, Bash scripting
    - GitHub Actions / CI pipelines
Example: GitOps with ArgoCD/Flux
Deploy and configure ArgoCD or Flux in the Cluster and create a basic pipeline to manage the application deployments from a source code repository to the Kubernetes environment. Document the process for synchronizing changes from the repository to the simulated production environment.

## 4. Security, best practices and testing/simulations (5 minutes)
- Understanding of security principles
    Zero Trust security model
- Best practices for system administration
    Password management on kubernetes and IaC
        vault
        1password
        github

## 5. Teamwork (5 minutes)
Teamwork in remote environments
Communicating with development and operations

## Part III. Questions

## 1. Candidate's questions (5 minutes)
Questions about the team, company or role

## 2. Conclusion (5 minutes)
Summarize the points discussed
Information on next steps
    Review of the application by our application team
    Next steps -> CEO interview
Thank the candidate for his or her time
