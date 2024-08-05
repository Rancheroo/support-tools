 # Collection Scripts - What we do and don't collect

This document provides an explanation of what data is collected by `cluster-collector.sh` and `nodes-collector.sh`. It also outlines what information each script collects, ensuring transparency and compliance with security and privacy practices.

## Table of Contents
1. Introduction
2. cluster-collector.sh
3. nodes-collector.sh
4. Security and Privacy Implications: What the Scripts Don't Collect

## Introduction
These scripts are designed to collect information from a cluster, which is crucial for understanding the system's configuration and performance. The data collected is anonymized and aggregated to protect individual node identities and personal information. This document aims to clarify what each script collects and how this information is used to enhance security and privacy.

**cluster-collector.sh Collected Information:**

* **General Cluster Info:**
    * Date and Time the script runs
    * Kubernetes version
    * List of nodes and their details (e.g., names)
    * List of namespaces
    * List of services (default and all namespaces, if `-A` flag is used with `kubectl`)
    * List of Custom Resource Definitions (CRDs)
    * List of DaemonSets (including those in `cattle-system`, `kube-system`, and `calico-system` namespaces, if applicable)
    * Total number of pods and their container counts
    * Pod distribution across nodes (number of pods per node)
    * List of pods in a "Terminating" state
    * List of invalid pods (based on container status checks)

* **Cluster Provider Specific Info:**
    * For RKE clusters: Network plugin details, etcd and kube-api configurations
    * For RKE2 clusters: Arguments passed to kube-apiserver, kube-controller-manager, kube-proxy, and kube-scheduler pods (redacted)
    * For RKE2 and k3s clusters: Redacted configuration files (if available)
    * For Harvester clusters: Deployments in the `harvester-system` namespace

* **Upstream Cluster Info (if `IS_UPSTREAM_CLUSTER` is set to "true"):**
    * Features installed
    * Bundle deployments
    * Deployments in the `cattle-system` namespace
    * Cattle controller configuration
    * Fleet local bundle (if available)
    * Installed Rancher version and number of pods
    * Rancher image ID (if available)
    * Cluster install UUID
    * Nodes managed by Cattle
    * Number of API tokens
    * Cluster role templates and cluster roles

* **Application Specific Info:**
    * DaemonSets in the `longhorn-system` namespace (if Longhorn is present)
    * Longhorn volumes (if available)

**Not Collected Information:**

* Individual pod details (except for status checks and names)
* Service details beyond names (unless `-o json` is used with `kubectl`)
* Specific container logs or configurations
* Network traffic details
* User or role information

**Additional Notes:**

* The script obfuscates sensitive information like tokens and image IDs if the `OBFUSCATE` environment variable is set to "true".
* The script creates a compressed archive (`clusterinfo.tar.gz`) containing the collected information unless debug mode (`DEBUG`) is enabled.
* The script handles errors and exits gracefully.

## nodes-collector.sh
`nodes-collector.sh` runs on every node of the cluster, collecting similar but more detailed information specific to each node:

##  **nodes-collector.sh Collected Information**

This script, designed to run on each node in a Kubernetes cluster, gathers system and network information for troubleshooting purposes. Here's a breakdown of its functionalities:

**Collecting System Information:**

* The `collect_systeminfo` function gathers details about:
    * Host filesystem information (`/host` prefixed paths)
    * System configuration files (e.g., `/etc/os-release`)
    * Processes running (`ps`)
    * Memory and disk usage (`free`, `df`)
    * Kernel settings (`sysctl`)
    * Kubectl version
    * Rancher server version (if applicable)

**Collecting Networking Information:**

* The `collect_networking_info` function gathers extensive network data, including:
    * Firewall rules (iptables)
    * Network interfaces and routes (`ip addr show`, `ip route show`)
    * Network connections (`ss`)
    * DNS resolution details (using dig)
    * Internal and external DNS checks for the Rancher server
    * CNI configuration files (if present)

**Cluster Provider Specific Information:**

* Depending on the cluster provider (`CLUSTER_PROVIDER` environment variable), the script collects additional details:
    * For RKE clusters: Network plugin info, etcd and kube-api configurations.
    * For RKE2 clusters: Arguments passed to control plane pods (redacted), configuration files (redacted if available).
    * For k3s clusters: Similar to RKE2, with k3s specific configurations.

**Upstream vs Downstream Cluster Handling:**

* The script differentiates between upstream and downstream clusters (based on `IS_UPSTREAM_CLUSTER`):
    * Upstream clusters: No specific commands are run on the node (currently).
    * Downstream clusters: No specific commands are defined yet (may be implemented later).

**Collecting Provider Specific Certificates:**

* Depending on the cluster provider, certificate information is collected:
    * For RKE: Certificates from `/opt/rke/etc/kubernetes/ssl` (if present).
    * For k3s: Agent and server certificates from `/var/lib/rancher/k3s`.
    * For RKE2: Agent and server certificates from the RKE2 data directory (determined by configuration or default location).
    * Certificates are saved after removing private key details using `openssl x509`.

**Cleaning Up Sensitive Information:**

* The `delete_sensitive_info` function removes kubeconfig files from the CNI configuration directory.

**Obfuscation:**

* If `OBFUSCATE` environment variable is set to "true":
    * A file "obfuscate_data" is created to indicate obfuscation.
    * Specific JSON and text files containing potentially sensitive information are obfuscated using external scripts (`obfuscate_json.py` and `obfuscate_text.py`, not included).

**Overall, this script provides a comprehensive mechanism for gathering system and network information from each node in a Kubernetes cluster, potentially aiding in troubleshooting and diagnostics.**

## Security and Privacy Implications: What the Scripts Don't Collect

While the provided scripts collect a substantial amount of system and network information, it's crucial to understand what they *don't* collect. This information can be equally important from a security and privacy perspective.

### Key Areas of Non-Collection:

1. **User-Specific Data:**
   * No personal identifiable information (PII) is collected.
   * No user activity logs or browsing history is tracked.
   * No user credentials or authentication data is captured.

2. **Sensitive Application Data:**
   * The scripts do not delve into the data processed by applications running on the cluster.
   * No content of files or databases is accessed or copied.
   * There's no exploration of application logs or configuration files beyond their existence.

3. **Network Traffic Content:**
   * Only network metadata (e.g., connections, ports, addresses) is collected.
   * No actual data packets are intercepted or analyzed.
   * There's no deep packet inspection or protocol analysis performed.

4. **Cryptography Keys:**
   * While the scripts collect certificate information, they do not extract private keys.
   * Cryptographic key material is not accessed or stored.

