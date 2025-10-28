# Prerequisites

## Hardware Requirements

This project can be run on any modern x86_64 system that meets the recommended system
requirements of [Proxmox](https://pve.proxmox.com/wiki/System_Requirements). I
recommend mini-SFF workstations such as those from [Project
TinyMiniMicro](https://www.servethehome.com/introducing-project-tinyminimicro-home-lab-revolution/).
Alternatively, you may choose to run the Proxmox cluster on your own PC for simple setup.

### My Current Setup

| Component | Specification | Details |
|-----------|---------------|---------|
| **Proxmox Node** | PC Gaming MS-7B19 | 1x Physical Server |
| **CPU** | Intel Core i5-8500 | 6 cores @ 3.00GHz, 1 socket, x86_64 |
| **RAM** | 16GB DDR4 | 2x 8GB modules |
| **Storage** | 1TB SSD | OS and VM storage |
| **Network Switch** | TP-Link 5 Port Gigabit | Network bridge configuration |
| **WiFi Integration** | TP-Link WiFi Adapter | Connected via bridge mode for wireless LAN |
| **Proxmox Version** | VE 8.4.1 | Current deployment version |

While a separate router and NAS is recommended, I run a virtualized instance of both within Proxmox itself.

## Proxmox Node

A Proxmox Virtual Environment (PVE) server is required to host the virtual machines and containers. For detailed installation instructions, see the [Proxmox Installation Guide](./proxmox_installation.md).


## Controller Node (Your Laptop)

A workstation/laptop/PC or separate host system will be used to run the
required provisioning tools. This system will need to have the following tools
installed:

| Tool | Purpose | Key Features | Installation Link |
|------|--------|--------------|-------------------|
| **Packer** | VM Image Building | • Automated VM template creation<br>• Multi-platform support (VMware, VirtualBox, etc.)<br>• Infrastructure as Code for images<br>• Integration with cloud providers | [Install Packer](https://developer.hashicorp.com/packer/install) |
| **Terraform** | Infrastructure Provisioning | • Declarative infrastructure management<br>• State management and tracking<br>• Multi-cloud and on-premises support<br>• Plan and apply workflow<br>• Resource dependency management | [Install Terraform](https://developer.hashicorp.com/terraform/install) |
| **Mdbook** | Documentation Generation | • Static site generation from Markdown<br>• Built-in search functionality<br>• Responsive design<br>• Syntax highlighting<br>• Table of contents generation | [Install Mdbook](https://rust-lang.github.io/mdBook/guide/installation.html) |

> **Note**: This documentation was tested with Packer v1.14.x and Terraform v1.13.x. While newer versions should work, please verify compatibility if you encounter any issues.


## Cluster Requirements

- An existing Proxmox server that is reachable by the controller node
- (Optional) An offline, private root and intermediate CA.
- A self-signed certificate, private key for TLS encryption of Vault.
