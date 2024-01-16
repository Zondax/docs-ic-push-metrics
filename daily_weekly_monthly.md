### Daily Tasks

**Infrastructure Monitoring**
   - Verify the health and status of all Kubernetes clusters.
   - Monitor resource consumption and service availability of Kubernetes clusters, including growth trends in resource usage.
   - Verify the health and performance of VMs/LXCs in Proxmox.
   - Review Prometheus, Grafana and Loki alerts and events.
   - Review Ceph status: Available and allocated space, S3 buckets.
   - Review Firewall status and logs.
   - Review status of automatic backups.

**Incident Response and Support**
   - Management and resolution of incidents reported to PagerDuty.
   - Perform troubleshooting of problems related to pods, services and deployments for own platform infrastructure. In case of problems related to development deployments or external users, notify them so they can correct it.
   - Attend and solve technical problems reported by development teams and external users (support channel and ClickUp).

**Log Review**
   - Analyze logs to detect anomalous behavior or potential improvements to platform-related deployments.

**Deployments and Updates**
   - Review the status of the GitHub Runners deployed in the infrastructure for CI execution.
   - Review the synchronization status of applications deployed on the clusters via CD (ArgoCD/Flux.CD). In case of problems related to development deployments or external users, notify them so that they can fix them.
   - Monitor and support the ongoing deployment of applications with ArgoCD/Flux.CD.
   - Notify about patches and software updates that may need to be applied in order to plan tasks accordingly.

**Configuration Management**
   - Ensure that all systems are configured correctly.
   - Check that secrets, configmaps and security policies are applied.

**Development Tasks**
   - Identify and automate repetitive operational tasks to increase efficiency and reduce the possibility of human error.
   - Review and work on platform development tasks assigned in ClickUp.


### Weekly Tasks

**Backups Review**
   - Verify integrity and status of backups.
   - Test data restoration procedure.

**Resource Optimization**
   - Evaluation of resources allocated/utilized in k8s cluster deployments.
   - Evaluation of allocated/utilized resources on servers (Proxmox).
   - Evaluation and autoscaling adjustment proposal.
   - Perform cleanup of unused or inactive resources.

**Key Metrics Assessment**
   - Review key metrics and service level indicators to ensure that targets and service level agreements agreed with external customers are met.

**Coordination Meetings**
   - Weekly meeting with development and operations team to see the status of projects and next needs that may arise.
   - Plan strategies to improve existing infrastructure.

**Security and Access Control Audit**. 
   - Review security policies applied to the clusters and plan possible changes to be made.
   - Perform security scans and plan possible changes at the Kubernetes configuration and container image level.
   - Review and update user access and control policies (Users, Groups, Passwords, 2FA and account status).

**Documentation**
   - Update technical documentation and operating procedures.
   - Update IPs/Hardware mapping.


### Monthly Tasks

**Infrastructure Performance Reports**
   - Generate and analyze reports on system utilization and performance.
   - Provide recommendations based on metrics and indicators.

**Resource Assessment**
   - Evaluation of resource utilization and optimization associated with clusters and VMs/LXCs for possible optimization.

**Disaster Recovery**
   - Perform disaster recovery testing to ensure that procedures and strategies are up to date and effective.

**Architecture Review**
   - Evaluate the efficiency and effectiveness of the current architecture.
   - Propose and plan infrastructure enhancements.

**Training and Continuous Improvement**
   - Presentation of past incidents to identify lessons learned and prevent similar problems from happening.
   - Establish time for self-training and training on new technologies and practices.
   - Attend webinars, courses or conferences relevant to CI/CD, Kubernetes and DevOps.

**Strategic Planning**
   - Participate in planning sessions for future projects and system scalability.

