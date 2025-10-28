# HashiCorp Vault Infrastructure

## Overview

This section covers the complete HashiCorp Vault infrastructure setup on Proxmox, including image building, server deployment, and component configuration. Vault provides secure secret management for your homelab environment.

## Components

### **Vault Image Builder**
Creates a custom Vault server image using Packer with KMS encryption support.

### **Vault Server**
Deploys the Vault server using Terraform with secure configuration.

### **Vault Components**
Configures Vault with authentication methods, policies, and secrets engines.

## Prerequisites

- Proxmox server running and accessible
- Packer installed for image building
- Terraform installed for infrastructure deployment
- AWS KMS key for Vault encryption
- Environment variables configured

## Quick Start

1. **Build Vault Image** - Create custom Vault server template
2. **Deploy Vault Server** - Deploy Vault server to Proxmox
3. **Configure Components** - Set up authentication and secrets

## Security Features

- **KMS Encryption**: AWS KMS integration for secure key management
- **TLS Communication**: Encrypted communication with client certificates
- **Access Control**: Role-based access control and policies
- **Secret Management**: Secure storage and retrieval of sensitive data

## Next Steps

- [Vault Image Builder](vault_image.md) - Create custom Vault server image
- [Vault Server Deployment](vault_server.md) - Deploy Vault server to Proxmox
- [Vault Components Setup](vault_components.md) - Configure Vault authentication and policies
