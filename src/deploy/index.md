# Homelab for Data Engineering - Deployment Documentations

## Overview

This repository provides a complete automation framework for Proxmox VE environments, combining Terraform for infrastructure provisioning and Ansible for configuration management. The solution supports complex multi-tier architectures with flexible inventory management and sequential deployment workflows.

## Key Features

- **🔧 Infrastructure as Code**: Complete Terraform automation for Proxmox VE
- **📦 Template Management**: Packer-based VM template creation and versioning
- **⚙️ Configuration Management**: Ansible playbooks with role-based deployment
- **🏗️ Modular Design**: Reusable components for different environments
- **📊 Dynamic Inventory**: Auto-generated Ansible inventories from Terraform state
- **🔄 Sequential Deployment**: Dependency-aware playbook execution
- **🎯 Multi-Architecture Support**: Complex cluster configurations (Spark, Kafka, etc.)


## Project Structure

The current repository structure is organized as follows:

```
proxmox-homelab-deploy/
├── ansible/                    # Configuration management
├── config/                     # Configuration files
├── terraform/                  # Infrastructure provisioning
│   ├── modules/               # Reusable Terraform modules
│   ├── deployments/           # Stack-specific deployments
│   └── templates/             # VM template creation
└── pyproject.toml             # Python project configuration
```
## Getting Started

Before implementing any stack, please read the following sections:

### 📋 General Guidelines
- **[General Guidelines](general_guidelines/index.md)** - Overview of deployment principles and best practices
- **[Set Environment](general_guidelines/set_environment.md)** - Environment setup and configuration
- **[Build Configuration](general_guidelines/build_config.md)** - Configuration management and templating
- **[Convention Rules](general_guidelines/convention_rules.md)** - Conventions for choosing VM IDs and IP Addresses


### Stacks
Choose a [stack](./stacks/index.md) from the available options to deploy.
