# Proxmox Setup

This section covers the essential Proxmox configuration and VM template setup for your homelab infrastructure.

## Proxmox Administration

Configure Proxmox users, roles, and storage for secure automation and infrastructure management.

### [Users and Roles](./users_and_roles.md)
Create dedicated automation users and custom roles for Terraform and Packer operations with appropriate permissions.

### [Local Storage](./local_storage.md)
Configure local storage for ISO images and container templates with automated downloads.

## VM Templates

Pre-configured VM templates for rapid deployment with cloud-init integration.

### [Ubuntu Server 22.04 LTS (Jammy)](./base_vm_template_ubuntu_server.md)
Ubuntu Server template with cloud-init support for automated configuration.

### Getting Started

1. Choose your desired template from the list above
2. Follow the template-specific documentation
3. Build the template using Packer
4. Deploy VMs from the created template