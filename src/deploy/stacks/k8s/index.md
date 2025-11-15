# Kubernetes Cluster Deployment

This guide walks you through deploying a Kubernetes cluster on Proxmox using Terraform and Ansible. The deployment consists of 4 main steps that must be executed in order.

## Prerequisites

Before starting, ensure you have:
- A running Proxmox VE server
- Vault configured with necessary secrets
- Terraform installed
- Ansible installed
- Completed [Hashicorp Vault setup](../../../initial_setup/vault/index.md)

## Set up environment variables

> Note: If you haven't completed this setup [Setting Environment Variables](../../general_guidelines/set_environment.md), please follow the instructions from this page before proceeding to the next steps.

## Build Template (k8s_base)

This step creates a base VM template with Kubernetes pre-installed that will be used to deploy the cluster.

```bash
cd terraform/templates/k8s_base
terraform init -backend-config="bucket=$TF_AWS_BACKEND_BUCKET" -backend-config="region=$TF_AWS_REGION"
terraform plan
terraform apply
```


## Update config/vms.yml

Configure your Kubernetes cluster by updating the VM specifications in the configuration file. For example:

```yaml
k8s_cluster:
  vms:
    k8s_controller:
      hosts:
        - name: "k8s-controller"
          id: 2010
          ip: "192.168.1.110"
          tags: ["vm", "k8s", "k8s_controller"]
          vars:
            role: "controller"
      template_id: 1004
      node: "pve2"

    k8s_node:
      hosts:
        - name: "k8s-node-1"
          id: 2011
          ip: "192.168.1.111"
          tags: ["vm", "k8s", "k8s_node"]
          vars:
            role: "node"

        - name: "k8s-node-2"
          id: 2012
          ip: "192.168.1.112"
          tags: ["vm", "k8s", "k8s_node"]
          vars:
            role: "node"

        - name: "k8s-node-3"
          id: 2013
          ip: "192.168.1.113"
          tags: ["vm", "k8s", "k8s_node"]
          vars:
            role: "node"
      template_id: 1004
      node: "pve2"
```

**Configuration options:**
- **k8s_controller**: Define Kubernetes controller node(s) with their IPs
- **k8s_node**: Define Kubernetes worker nodes with their IPs
- **Node**: Proxmox node name where the VM will be deployed (e.g., "pve", "pve2")
- **Template ID**: Must reference the ID of the template
- **IP Addresses**: Ensure they don't conflict with existing VMs
- **VM IDs**: Must be unique across your Proxmox environment

## Deploy VM with Terraform (k8s/)

Deploy the actual Kubernetes cluster using the configuration from Step 3.

```bash
cd terraform/deployments/k8s
terraform init -backend-config="bucket=$TF_AWS_BACKEND_BUCKET" -backend-config="region=$TF_AWS_REGION"
terraform plan
terraform apply
```

**What this does:**
- Creates Kubernetes controller VM(s) from the template
- Creates Kubernetes worker node VM(s) from the template
- Configures network settings and cloud-init
- Runs Ansible playbooks to configure Kubernetes services
- Initializes the controller node with kubeadm
- Joins worker nodes to the cluster

**Deployment includes:**
- **Kubernetes Controller**: Control plane node with API server, etcd, and scheduler
- **Kubernetes Nodes**: Worker nodes that join the cluster
- **Ansible Configuration**: Automated setup of Kubernetes services
- **Network Configuration**: Proper gateway and IP assignment
- **Pod Network**: Flannel CNI with CIDR 10.244.0.0/16

## Verification

After deployment, verify your Kubernetes cluster:

1. **Check VM Status**: Ensure all VMs are running in Proxmox
2. **SSH into Controller**: Connect to the controller node
3. **Check Cluster Status**: Run `kubectl get nodes` to verify all nodes are ready
4. **Check System Pods**: Run `kubectl get pods -n kube-system` to verify system components
5. **Test Deployment**: Deploy a test pod to verify cluster functionality

**Example verification commands:**

```bash
# SSH into the controller node
ssh <username>@<k8s-controller-ip>

# Check node status
kubectl get nodes

# Check all pods in kube-system namespace
kubectl get pods -n kube-system

# Get cluster information
kubectl cluster-info

# Deploy a test nginx pod
kubectl run nginx-test --image=nginx --restart=Never
kubectl get pods
```

**Expected output:**
- All nodes should show status `Ready`
- System pods (kube-proxy, flannel, etc.) should be running
- Test pod should be created and running

## Cleanup

To remove the Kubernetes cluster:

```bash
cd terraform/deployments/k8s
terraform destroy
```

To remove the base template:

```bash
cd terraform/templates/k8s_base
terraform destroy
```

**Note**: This will permanently delete all VMs and templates. Ensure you have backups if needed. Before destroying, you may want to drain and delete nodes gracefully:

```bash
# On the controller node
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
kubectl delete node <node-name>
```
