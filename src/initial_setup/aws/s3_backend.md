# S3 Backend (Terraform State Storage)

This module provisions the S3 bucket used as the remote backend for Terraform state across the project.

## Prerequisites

- An AWS account with permissions to create S3 buckets.
- AWS authentication configured (see "AWS Backend Configuration" page for methods and verification).

## Configuration
### Module Location

`resources/aws/00_tf_s3_backend`

### Configure Variables

Copy and edit variables:

```bash
cd resources/aws/00_tf_s3_backend
cp terraform.tfvars.example terraform.tfvars
```

Set the following in `terraform.tfvars`:

| Field | Type | Description | Example |
|---|---|---|---|
| `aws_region` | string | AWS region for resources | "us-east-1" |
| `s3_bucket_name` | string | Name of the S3 bucket for Terraform backend | "terraform-backend-proxmox" |
| `admin_users` | list(string) | IAM principal ARNs with admin access to the S3 bucket | ["arn:aws:iam::<acct>:user/admin1"] |
| `standard_users` | list(string) | IAM principal ARNs with standard access to the S3 bucket | ["arn:aws:iam::<acct>:user/user1"] |

The lists `admin_users` and `standard_users` must each contain at least one ARN.

## Deploy

```bash
terraform init
terraform plan
terraform apply
```

## Outputs

- `s3_bucket_name`: The name of the S3 bucket for Terraform backend
- `s3_bucket_arn`: The ARN of the S3 bucket for Terraform backend
- `s3_bucket_domain_name`: The bucket domain name for Terraform backend

## Use This Bucket as Remote Backend

Other modules (for example `resources/aws/01_kms`) read backend settings from a `backend.config` file. After the bucket is created, set its name in that file:

```hcl
# resources/aws/01_kms/backend.config
bucket = "<your-created-bucket-name>"
region = "<your-region>"
```

Then initialize the downstream module with:

```bash
terraform init -backend-config=backend.config
```

## Cleanup

To remove the backend bucket, ensure it is empty first (Terraform state files are stored here). Then:

```bash
terraform destroy
```
