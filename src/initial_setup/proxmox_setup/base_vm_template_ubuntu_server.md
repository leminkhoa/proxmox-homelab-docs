# Proxmox Base VM Template - Ubuntu Server 22.04 LTS (Jammy)

## Overview

This configuration creates a standardized Ubuntu Server 22.04 LTS (Jammy) VM template for Proxmox using Packer. The template is optimized for cloud-init integration and provides a consistent foundation for VM deployments across your Proxmox homelab environment.

## Prerequisites

- Proxmox server is running and accessible
- Packer is installed on your build machine
- Proxmox API token with appropriate permissions
- SSH key pair for authentication
- Sufficient disk space on Proxmox node (minimum 20GB)
- Network connectivity between build machine and Proxmox server


## VM Specifications

| Component | Specification |
|-----------|--------------|
| **OS** | Ubuntu Server 22.04 LTS (Jammy) |
| **CPU** | 4 cores |
| **Memory** | 4 GB |
| **Disk** | 20 GB (virtio) |
| **Network** | virtio (vmbr0) |
| **Storage Pool** | local-lvm |
| **VM ID** | 1000 (configurable) |


## Configuration

### Module Location

```bash
resources/proxmox/01_base_vm_template/ubuntu_server_jammy
```

### Files
`build.pkr.hcl` - Build Definition
The main build configuration that defines:
- **Build name**: `ubuntu-server-jammy`
- **Source reference**: Links to the Proxmox source configuration
- **Provisioning steps**: Shell commands for VM preparation
- **File provisioning**: Cloud-init configuration files
- **Template optimization**: Removes machine-specific data for templating

`sources.pkr.hcl` - Source Configuration
Defines the Proxmox VM source with:
- **Connection settings**: Proxmox API URL, authentication
- **VM specifications**: CPU, memory, disk, network settings
- **ISO configuration**: Ubuntu Server 22.04 LTS installation
- **Cloud-init setup**: Automated installation and configuration
- **Boot commands**: Automated installation parameters

`variables.pkr.hcl` - Variable Definitions
Contains configurable parameters:
- **Proxmox connection**: Host IP, API tokens, SSH credentials
- **VM settings**: Node name, VM ID, storage pools
- **Validation rules**: Input validation for security and correctness

### Set Environment Variables

Configure the required environment variables for Packer:
| Variable | Description | Type | Default | Required |
|----------|-------------|------|---------|----------|
| `proxmox_host_ip` | Proxmox server URL | `string` | `env("PROXMOX_HOST_IP")` | Yes |
| `proxmox_api_token_id` | API token ID | `string` | `env("PROXMOX_API_TOKEN_ID")` | Yes |
| `proxmox_api_token_secret` | API token secret | `string` | `env("PROXMOX_API_TOKEN_SECRET")` | Yes |
| `ssh_username` | SSH username for build | `string` | `env("SSH_USERNAME")` | Yes |
| `ssh_public_key` | Public key used to authorize users to the VM | `string` | `env("SSH_PUBLIC_KEY")` | Yes |
| `vm_id` | VM ID for the template (1000-1999) | `number` | `env("VM_ID")` | No |


As those variables refer values from environment variables value, we can set it directly from command line. For example:

```bash
# Proxmox Connection
export PROXMOX_HOST_IP="https://192.168.1.100"
export PROXMOX_API_TOKEN_ID='terraform@pve!deployment-token'
export PROXMOX_API_TOKEN_SECRET="your-token-secret"
export SSH_USERNAME="your-ssh-user"
export SSH_PUBLIC_KEY=$(cat ~/.ssh/id_rsa.pub)
export PKR_VAR_vm_id=1000
```

> **Note**: The SSH public key path can vary depending on your key type and location:
> - **RSA keys**: `~/.ssh/id_rsa.pub`
> - **ED25519 keys**: `~/.ssh/id_ed25519.pub` (recommended)
> - **ECDSA keys**: `~/.ssh/id_ecdsa.pub`
> - **Custom location**: Use the full path to your public key file

### Customize Configuration (Optional)

Edit the following files as needed:

**`http/user-data.tpl`** - Cloud-init user configuration:
- Update user information
- Modify SSH keys
- Adjust timezone settings
- Add/remove packages

**`files/99-pve.cfg`** - Proxmox-specific cloud-init settings:
- Configure datasource for Proxmox integration

### Build the Template

Before building, validate your configuration to ensure everything is properly configured:

```bash
packer validate .
```

If validation passes, run Packer to build the VM template:

```bash
packer build .
```

To update image, you can run build with `-force` argument:
```bash
packer build -force .
```

For more information about using variables with Packer, see the [Packer build command documentation](https://developer.hashicorp.com/packer/docs/commands/build).

