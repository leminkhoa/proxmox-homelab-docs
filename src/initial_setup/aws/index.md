# AWS Backend Configuration

## Overview

This section covers the AWS backend configuration required for our Proxmox homelab infrastructure. The AWS backend provides secure, scalable storage and encryption services that are essential for managing our infrastructure state and sensitive data.

## Why AWS Backend is Essential

Before deploying our Proxmox homelab infrastructure, we need to establish a secure AWS backend for several critical reasons:

### Enhanced Security
- **Encrypted State Storage**: Terraform state files contain sensitive information including resource IDs, IP addresses, and configuration details. AWS S3 with server-side encryption ensures these secrets are protected at rest.
- **KMS Key Management**: AWS KMS provides centralized key management with automatic rotation, audit logging, and fine-grained access controls for encrypting/decrypting sensitive data.
- **Access Control**: IAM policies ensure only authorized users can access the backend resources, preventing unauthorized infrastructure modifications.

### Operational Benefits
- **Remote State Management**: Multiple team members can collaborate on infrastructure changes with consistent state locking.
- **Backup and Recovery**: Automated backups and point-in-time recovery capabilities for both Terraform state and Vault data.
- **Cost Optimization**: Pay-as-you-use model with lifecycle policies to automatically transition data to cheaper storage classes.

## Components

The AWS backend for this project is split into two modules:

- [S3 Backend](./s3_backend.md): Remote Terraform state storage
- [KMS Backend](./kms_backend.md): Encryption key management (auto-unseal, storage encryption)

Use the S3 module first to create the remote state bucket, then configure the KMS module to use that bucket via `backend.config`.

## AWS Authentication

Authenticate with AWS using one of the following methods:

```bash
# Environment variables (recommended)
export AWS_ACCESS_KEY_ID="<access-key-id>"
export AWS_SECRET_ACCESS_KEY="<secret-access-key>"
export AWS_DEFAULT_REGION="us-east-1"
```

```ini
# ~/.aws/credentials (credentials file)
[default]
aws_access_key_id = <access-key-id>
aws_secret_access_key = <secret-access-key>
region = us-east-1
```

```bash
# AWS named profile (example)
aws configure --profile proxmox-backend
export AWS_PROFILE=proxmox-backend
```

Verify credentials:

```bash
aws sts get-caller-identity
```
