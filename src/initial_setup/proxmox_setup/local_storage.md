# Proxmox Local Storage Configuration

This configuration sets up local storage for ISO images and container templates on your Proxmox server. It includes automated downloads of Ubuntu server images and Debian container templates, providing the foundation for VM and container creation.

## Prerequisites

- Proxmox server is running and accessible
- Terraform provider for Proxmox is configured
- Administrative access to Proxmox server
- Users and roles configuration completed (see [Users & Roles](./users_and_roles.md))
- Sufficient disk space for ISO images and templates
- Network connectivity for downloading images

> **Important**: Ensure you have completed the Users and Roles configuration before proceeding with local storage setup. The terraform user created in the previous steps will be used for automated operations.

## Configuration

### Module Location

`resources/proxmox/00_proxmox_administration/01_local_storage`

### Configure Backend

Copy the backend configuration example and update the values:

```bash
cp backend.config.example backend.config
```

Edit `backend.config` and update the S3 bucket name:

```hcl
bucket = "your-actual-s3-bucket-name"
region = "us-east-1"
```

### Configure Variables

The following variables will be prompted during deployment:

| Variable | Type | Description | Default | Required |
|---|---|---|---|---|
| `proxmox_endpoint` | string | The URL of the Proxmox endpoint | `"https://192.168.1.100:8006"` | No |
| `user_terraform_password` | string | Password for terraform user authentication | - | Yes |
| `node_name` | string | Node name to deploy resources | `"pve"` | No |
| `data_store_id` | string | Data store ID for storage | `"local"` | No |

## Deploy

```bash
terraform init -backend-config="./backend.config"
terraform plan
terraform apply
```

> **Note**: From this step, use the `terraform@pve` user for automation instead of the standard user. This user has the necessary permissions for automated operations and follows security best practices.

## Storage Configuration

### Local Storage Setup

**Storage ID**: `local` (default)
**Node**: `pve` (default)
**Type**: Local storage for ISOs and templates

### Storage Requirements

**Minimum Space Requirements**:
- Ubuntu ISO: ~1.5 GB
- Debian Template: ~200 MB
- **Total**: ~2 GB minimum

**Recommended Space**:
- **Minimum**: 5 GB for current downloads
- **Recommended**: 10 GB for future templates and ISOs
- **Production**: 20+ GB for multiple OS versions

> **Tip**: Consider using a dedicated storage pool for templates and ISOs to avoid filling up your main storage. This also makes it easier to manage and backup your template library.


