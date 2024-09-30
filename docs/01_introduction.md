# IC metrics & logs

## Introduction

The current state of Internet Computer Protocol (ICP) nodes lacks open access to metrics, relying predominantly on pull-based approaches. This method necessitates modifications at the firewall level, potentially introducing security vulnerabilities.

Our project aims to address these limitations by modifying the IC node source code to allow nodes to push metrics. This eliminates the need for firewall reconfiguration and external IP management, providing a more secure and streamlined method for accessing node metrics.

This project is designed to significantly benefit IC Node providers, offering an easier and more secure way to obtain metrics without altering firewall settings. Additionally, it introduces the capability for IC nodes to push metrics to configurable external endpoints. This means node providers can receive metrics on their own servers without needing to enable pull-based metrics collection.

The proposed changes to the IC node source code can be reviewed in detail [here](https://github.com/Zondax/ic/tree/metrics) , and we have submitted a preliminary [PR](https://github.com/dfinity/ic/pull/1727) for contributing to IC Nodes. 

A demo video is available [here](https://www.youtube.com/watch?v=ZG1-IJtHBzk)

Our project aims to improve how metrics and logs are managed within IC nodes, making the entire process more secure, customizable, and efficient for node providers.

### Current architecture

This is the current metrics and logs architecture deployed at IC nodes.

![Current architecture](./assets/IC_metrics_current_architecture.jpg)

The current architecture is using distinct software for managing logs and metrics at IC nodes. The logs are pushed to external elasticsearch servers using filebeat and the metrics are pulled from known victoria metrics servers that are allowed to do it.  

### Propossed architecture

These are the main goals of this project:
1. Unify the software used to ship metrics and logs to [vector](https://vector.dev/).
1. Allow the chance to push metrics from IC nodes to its provider, in a prometheus exporter format (prometheus or victoriametrics).

![Data Flow architecture](./assets/IC_metrics_proposed_architecture.jpg)
