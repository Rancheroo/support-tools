 # Data Collection Explanation

This document provides an explanation of what data is collected by `cluster-collector.sh` and `nodes-collector.sh`. It also outlines what information each script collects, ensuring transparency and compliance with security and privacy practices.

## Table of Contents
1. [Introduction](#introduction)
2. [cluster-collector.sh](#cluster-collectorsh)
3. [nodes-collector.sh](#nodes-collectorsh)
4. [Security and Privacy Considerations](#security-and-privacy-considerations)
5. [Conclusion](#conclusion)

## Introduction
These scripts are designed to collect information from a cluster, which is crucial for understanding the system's configuration and performance. The data collected is anonymized and aggregated to protect individual node identities and personal information. This document aims to clarify what each script collects and how this information is used to enhance security and privacy.

## cluster-collector.sh
`cluster-collector.sh` is a script that runs on the master node of the cluster, collecting various system and configuration details. Here's an overview of what it collects:

### Collected Information:
- **System Information**: 
  - Hostname
  - Operating System version
  - Kernel version
  - CPU architecture
  - Memory information
  - Disk space usage
  - Network interfaces and IP addresses
  
- **Software Inventory**:
  - Installed software packages
  - Version numbers of installed software
  
- **Configuration Files**:
  - Configuration files relevant to the cluster setup (e.g., Kubernetes configuration, network configurations)
  
- **Performance Metrics**:
  - CPU usage
  - Memory usage
  - Disk I/O rates
  - Network traffic statistics

### What is Not Collected:
- **User Data**: Personal information, user accounts, or authentication tokens are not collected.
- **Sensitive Files**: System log files, application data logs, or other sensitive configuration files are not included in the collection.
- **Intermediate User Inputs**: Scripts do not capture any intermediate inputs from users during execution.

## nodes-collector.sh
`nodes-collector.sh` runs on every node of the cluster, collecting similar but more detailed information specific to each node:

### Collected Information:
- **System Information**:
  - Hostname
  - Operating System version
  - Kernel version
  - CPU architecture
  - Memory information
  - Disk space usage
  - Network interfaces and IP addresses
  
- **Software Inventory**:
  - Installed software packages
  - Version numbers of installed software
  
- **Configuration Files**:
  - Configuration files relevant to the node's role in the cluster (e.g., Docker configuration, application-specific configurations)
  
- **Performance Metrics**:
  - CPU usage
  - Memory usage
  - Disk I/O rates
  - Network traffic statistics

### What is Not Collected:
- **User Data**: Personal information, user accounts, or authentication tokens are not collected.
- **Sensitive Files**: System log files, application data logs, or other sensitive configuration files are not included in the collection.
- **Intermediate User Inputs**: Scripts do not capture any intermediate inputs from users during execution.

## Security and Privacy Considerations
The primary goal is to ensure that collected information does not contain personally identifiable information (PII) or sensitive data. The scripts are designed with security and privacy in mind, adhering to the following principles:
- **Anonymization**: Collected data is anonymized where possible, ensuring that individual node identities are protected.
- **Limited Scope**: Only necessary system and configuration details are collected, avoiding unnecessary exposure of user data or sensitive files.
- **Transparency**: Users are informed about what data is collected and how it will be used, in line with the principles outlined in the README.md file provided.

## Conclusion
The scripts `cluster-collector.sh` and `nodes-collector.sh` collect essential system and configuration information to aid in cluster management, performance monitoring, and security audits. The data collected is anonymous and does not include personal user information or sensitive files. This approach ensures compliance with privacy regulations while providing valuable insights for operational efficiency.

For more detailed information on how the data will be used and stored, please refer to the README.md file provided in the repository.
