# Vault Image Builder

## Overview

Creates a custom HashiCorp Vault server image for Proxmox using Packer. The image includes Vault pre-installed with KMS encryption support and systemd service configuration.

## Prerequisites

- Packer installed
- Proxmox server accessible
- [VM Templates](../vm_templates/index.md) setup completed
- [Proxmox Administration](../proxmox_administration/index.md) setup completed
- [Trusted CA Store](../controller/trusted_ca_store.md) configuration completed

## VM Specifications

| Component | Specification |
|-----------|--------------|
| **Base Template** | Ubuntu Server 22.04 LTS (Jammy) |
| **CPU** | 2 cores |
| **Memory** | 2 GB |
| **Disk** | 20 GB (virtio) |
| **Network** | virtio (vmbr0) |
| **Storage Pool** | local-lvm |

## Configuration

### Module Location

```bash
resources/proxmox/02_vault/00_vault_image
```

### Files

**`build.pkr.hcl`** - Build Definition
- Defines the build process and provisioning steps
- Configures Vault installation and service setup

**`sources.pkr.hcl`** - Source Configuration
- Proxmox VM source configuration
- Hardware specifications and network settings
- Cloud-init integration

**`variables.pkr.hcl`** - Variable Definitions
- Configurable parameters for the build
- Validation rules for input parameters

### Set Environment Variables

Configure the required environment variables for Packer:

| Variable | Description | Type | Default | Required |
|----------|-------------|------|---------|----------|
| `proxmox_host_ip` | IP address of Proxmox host | `string` | `"https://192.168.1.100"` | No |
| `proxmox_api_token_id` | API token ID in format `user@realm!tokenname` | `string` | - | Yes |
| `proxmox_api_token_secret` | API token secret for Proxmox authentication | `string` | - | Yes |
| `vm_id` | VM ID for the template ((should range between 1000 and 1999)) | `number` | - | Yes |
| `vm_name` | Name of the Vault template | `string` | - | Yes |
| `vm_ip` | IP address for the Vault VM | `string` | - | Yes |
| `ssh_username` | SSH username for build | `string` | - | Yes |
| `vault_version` | Vault version to install | `string` | `"1.20.4"` | No |
| `vault_cluster_name` | Name of the Vault cluster | `string` | `"my-vault"` | No |
| `vault_storage_node_id` | Storage node ID for Vault | `string` | `"vault-server"` | No |
| `kms_key_id` | AWS KMS key ID for Vault encryption | `string` | - | Yes |
| `aws_access_key_id` | AWS access key for KMS operations | `string` | - | Yes |
| `aws_secret_access_key` | AWS secret key for KMS operations | `string` | - | Yes |
| `aws_region` | AWS region for KMS operations | `string` | `"us-east-1"` | No |
| `tls_allowed_ips` | Comma-separated list of IP addresses for TLS certificate | `string` | - | Yes |

Set the required Packer variables using the `PKR_VAR_` prefix, for example:

```bash
# Packer Variables
export PKR_VAR_proxmox_host_ip="https://192.168.1.100"
export PKR_VAR_proxmox_api_token_id='terraform@pve!deployment-token'
export PKR_VAR_proxmox_api_token_secret="your-token-secret"
export PKR_VAR_vm_id=1002
export PKR_VAR_vm_name="vault-template"
export PKR_VAR_vm_ip="192.168.1.3"
export PKR_VAR_ssh_username="ubuntu"
export PKR_VAR_vault_cluster_name="my-vault"
export PKR_VAR_vault_storage_node_id="vault-server"
export PKR_VAR_kms_key_id="12345678-abcd-1234-abcd-123456789101"
export PKR_VAR_aws_access_key_id="your-aws-access-key"
export PKR_VAR_aws_secret_access_key="your-aws-secret-key"
export PKR_VAR_aws_region="us-east-1"
export PKR_VAR_tls_allowed_ips="127.0.0.1,192.168.1.3"
```

### Build the Image

Before building, validate your configuration:

```bash
packer validate .
```

If validation passes, run Packer to build the Vault image:

```bash
packer build .
```

To update image, you can run build with `-force` argument:
```bash
packer build -force .
```

## Post Deployment

### Client Certificate Management

After successful deployment, a **`client.pem`** file will be created in the `downloads/` folder. This certificate is essential for secure communication with your Vault server and should be moved to a secure location immediately.

**Purpose of the client certificate:**
- Enables secure TLS communication with the Vault server
- Required for authenticated API access to Vault
- Provides client authentication for encrypted connections
- Essential for future Vault operations and management

**Security Recommendations:**
1. Move `client.pem` to a secure location outside the downloads folder
2. Store in encrypted storage or secure key management system
3. Set appropriate file permissions (600) to restrict access
4. Keep backup copies in separate secure locations
5. Never commit certificate files to version control
