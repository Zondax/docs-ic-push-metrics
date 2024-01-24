# Technical Exercise for Mid/Senior SRE Engineer Candidates

## Objectives
Candidates will be required to demonstrate their ability to:

- Manage and orchestrate containers with Kubernetes.
- Use Terraform to create infrastructure as code.
- Monitor services using Prometheus Stack.
- Deploy packages using Helm Charts.
- Manage continuous integration and deployment (CI/CD) flows with GitOps (ArgoCD/Flux).

## Exercise Description
1. Infrastructure as Code with Terraform

Create a Terraform script to provision a Kubernetes cluster on a cloud provider (can be on-premises k3s or MiniKube or any other Cloud provider with a free tier such as GCP, AWS, Azure).
Ensure that the Terraform script follows best practices in terms of files, variables, locals and functionality.

2. Application Deployment with Helm and Kubernetes

Create a Helm Chart for a simple web application (it can be an Nginx web server with a static welcome page or similar).
Include in the Chart different configurations for separate deployments per environment (e.g. development, pre-production and production).
Deploy the application in the cluster created with Helm with the simulated production environment.

3. Monitoring with Prometheus

Deploy and configure Prometheus Stack in the Cluster to monitor the Kubernetes cluster and the deployed application.
Expose custom metrics from the web application and ensure that Prometheus collects them.

4. GitOps with ArgoCD/Flux

Deploy and configure ArgoCD or Flux in the Cluster and create a basic pipeline to manage the application deployments from a source code repository to the Kubernetes environment.
Document the process for synchronizing changes from the repository to the simulated production environment.

## Delivery and Evaluation
Code should be delivered in a GitHub repository with clear instructions in the README.md file on how to execute each step.
The rationale behind design decisions and any relevant security and best practice considerations should be provided.
Terraform scripts and Helm Charts should be structured in a modular and maintainable way.
The reliability of the CI/CD pipeline and how it handles incremental changes and desired state vs. current state reconciliation shall be evaluated.

## Guidelines for Candidate
Document the process followed, the challenges encountered and how they were solved using Markdown in a file in the repository.
Be sure to test your code in a clean environment to confirm that the installation steps are reproducible.
Pay attention to security details and best practices throughout the exercise.

## Recommended Time
We think approximately 3-4 hours working on this task will be enough, distributed among the different sections as needed. It is important that the time is sufficient to demonstrate the technical skills but also to present a well-organized and documented Markdown file.
