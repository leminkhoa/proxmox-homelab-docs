# Prerequisites

## Hardware Requirements

This project can be run on any modern x86_64 system that meets the recommended system
requirements of [Proxmox](https://pve.proxmox.com/wiki/System_Requirements). I
recommend mini-SFF workstations such as those from [Project
TinyMiniMicro](https://www.servethehome.com/introducing-project-tinyminimicro-home-lab-revolution/).
Alternatively, you may choose to run the Proxmox cluster on your own PC for simple setup.

### My Current Setup

**Cluster Name:** Homelab (2 nodes)

| Component | Node1 (pve) | Node2 (pve2) |
|-----------|-------------|--------------|
| **Hardware** | PC Gaming MS-7B19 | ASUSTeK COMPUTER INC - ELM_PN54 |
| **CPU** | Intel Core i5-8500 | AMD Ryzen AI 7 350 w/ Radeon 860M |
| **CPU Details** | 6 cores @ 3.00GHz, 1 socket, x86_64 | 16 cores (8 cores, 2 threads/core), 1 socket, x86_64 |
| **RAM** | 16GB DDR4 (2x 8GB modules) | 96GB DDR5 (2x 48GB modules @ 5600 MT/s) |
| **Storage** | 1TB SSD | 2TB SSD |
| **Kernel Version** | Linux 6.14.11-4-pve (2025-10-10T08:04Z) | Linux 6.14.11-4-pve (2025-10-10T08:04Z) |
| **Proxmox Version** | VE 9.0.11 | VE 9.0.11 |

**Shared Network Infrastructure:**
- **Network Switch:** TP-Link 5 Port Gigabit (Network bridge configuration)
- **WiFi Integration:** TP-Link WiFi Adapter (Connected via bridge mode for wireless LAN)

## Proxmox Node

A Proxmox Virtual Environment (PVE) server is required to host the virtual machines and containers. For detailed installation instructions, see the [Proxmox Installation Guide](./proxmox_installation.md).


## Controller Node (Your Laptop)

A workstation/laptop/PC or separate host system will be used to run the
required provisioning tools. This system will need to have the following tools
installed:

| Tool | Purpose | Key Features | Installation Link | Additional Modules |
|------|--------|--------------|-------------------|-------------------|
| **Packer** | VM Image Building | • Automated VM template creation<br>• Multi-platform support (VMware, VirtualBox, etc.)<br>• Infrastructure as Code for images<br>• Integration with cloud providers | [Install Packer](https://developer.hashicorp.com/packer/install) | |
| **Terraform** | Infrastructure Provisioning | • Declarative infrastructure management<br>• State management and tracking<br>• Multi-cloud and on-premises support<br>• Plan and apply workflow<br>• Resource dependency management | [Install Terraform](https://developer.hashicorp.com/terraform/install) | tfvenv |
| **Ansible** | Configuration Management | • Agentless architecture (SSH-based)<br>• Idempotent operations<br>• Playbook and role-based automation<br>• YAML-based configuration<br>• Inventory management<br>• Multi-node orchestration | [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html) | |
| **Mdbook** | Documentation Generation | • Static site generation from Markdown<br>• Built-in search functionality<br>• Responsive design<br>• Syntax highlighting<br>• Table of contents generation | [Install Mdbook](https://rust-lang.github.io/mdBook/guide/installation.html) | |

> **Note**: This documentation was tested with Packer v1.14.x and Terraform v1.13.x. While newer versions should work, please verify compatibility if you encounter any issues.


## Cluster Requirements

- An existing Proxmox server that is reachable by the controller node (your laptop)
- A self-signed certificate, private key for TLS encryption of Vault.
- (Optional) An offline, private root and intermediate CA.
