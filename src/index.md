# Proxmox Homelab Setup

This repository contains infrastructure-as-code for the automated deployment and configuration of a secure homelab environment on Proxmox, featuring HashiCorp Vault for secrets management and AWS integration for backend storage.

## Disclaimer

This project is in development status and subject to breaking changes.

Please do not run any code on your machine without understanding the provisioning flow, in case of data loss. Some configurations may perform destructive actions that are irreversible!

## Overview

This project provides a **semi-automated** approach to setting up a secure homelab infrastructure using modern DevOps practices:

1. **Packer** creates base Proxmox VM templates from cloud images and ISOs
2. **Terraform** provisions infrastructure components and manages Proxmox resources
3. **HashiCorp Vault** provides secure secrets management with AWS KMS encryption
4. **AWS Integration** for backend storage and encryption key management

The setup focuses on security, automation, and best practices for homelab environments.

## Features

- [x] Golden image creation with Packer for Ubuntu Server templates
- [x] Declarative Proxmox infrastructure management with Terraform
- [x] HashiCorp Vault deployment with AWS KMS encryption
- [x] Secure secrets management and retrieval
- [x] Proxmox user and role management
- [x] Local storage configuration
- [x] AWS S3 backend for Vault with KMS encryption
- [x] Automated certificate management
- [x] Infrastructure as Code best practices

## Getting Started

This repository's documentation is built with [mdbook](https://rust-lang.github.io/mdBook/), a Rust-based documentation engine for simplicity and speed.

To start your journey, navigate to [Getting Started](./getting_started.md) and follow the instructions.


## Acknowledgements

- [kencx/homelab](https://github.com/kencx/homelab/blob/master/README.md) - Inspiration for HashiCorp stack architecture
- [ChristianLempa/homelab](https://github.com/ChristianLempa/homelab/tree/main/proxmox) - Proxmox automation patterns
