# Getting Started

This guide provides the complete deployment sequence for a secure homelab environment on Proxmox with HashiCorp Vault and AWS integration.

## Prerequisites

Ensure all [prerequisites](./prerequisites.md) are met before proceeding.


## Quick Reference

| Step | Component | Purpose |
|------|-----------|---------|
| 1 | Trusted CA Store Configuration | Certificate trust configuration |
| 2 | AWS Backend | S3 storage and KMS encryption |
| 3 | Proxmox Admin | Users, roles, and storage setup |
| 4 | VM Templates | Base images for deployment |
| 5 | Vault Server | Secrets management infrastructure |

Follow each step sequentially for a complete homelab deployment.

## Deployment Sequence

### 1. Trusted CA Store Configuration
Configure the controller node to trust Proxmox certificates for secure Terraform operations.

**Guidelines:** [Trusted CA Store Configuration](./controller/trusted_ca_store.md)

### 2. AWS Backend Infrastructure
Deploy AWS S3 and KMS infrastructure for secure state storage and encryption.

**Guidelines:** [AWS Configuration](./aws/index.md)

### 3. Proxmox Administration
Configure Proxmox users, roles, and storage for automation.

**Guidelines:** [Proxmox Setup](./proxmox_setup/index.md)

### 4. VM Template Creation
Build base VM templates using Packer for consistent deployments.

**Guidelines:** [Base VM Templates](./proxmox_setup/base_vm_template_ubuntu_server.md)

### 5. Vault Infrastructure
Deploy and configure HashiCorp Vault with AWS KMS encryption.

**Guidelines:** [HashiCorp Vault](./vault/index.md)
