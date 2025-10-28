# KMS Backend (Encryption Key Management)

This module provisions an AWS KMS key and alias for encrypting sensitive data, such as Vault storage and other AWS services.

## Prerequisites

- S3 backend bucket created and available (see [S3 Backend](s3_backend.md) page).
- AWS authentication configured (see "[AWS Configuration](index.md#aws-authentication)" page for methods and verification).

## Configuration

### Module Location

`resources/aws/01_kms`

### Configure Backend

Copy the example backend config, then edit it:

```bash
cp backend.config.example backend.config
```

Update `backend.config` to point to your S3 state bucket:

```hcl
bucket = "<your-created-bucket-name>"
region = "<your-region>"
```

Initialize with:

```bash
terraform init -backend-config=backend.config
```

### Configure Variables

Copy and edit variables:

```bash
cp terraform.tfvars.example terraform.tfvars
```

Set the following in `terraform.tfvars`:

| Field | Type | Description | Example |
|---|---|---|---|
| `kms_alias_name` | string | Alias name for the KMS key | "vault-backend" |
| `kms_key_description` | string | Description for the KMS key | "KMS key for Vault backend encryption" |
| `kms_key_deletion_window` | number | Deletion window in days (7-30) | 30 |
| `kms_key_rotation` | bool | Enable automatic key rotation | true |
| `admin_users` | list(string) | IAM ARNs with full KMS permissions | ["arn:aws:iam::<acct>:user/admin1"] |
| `standard_users` | list(string) | IAM ARNs with encrypt/decrypt permissions | ["arn:aws:iam::<acct>:user/user1"] |

Validation rules enforce at least one admin and one standard user and that deletion window is between 7 and 30.

## Deploy

```bash
terraform plan
terraform apply
```

## Outputs

- `kms_key_id`: The globally unique identifier for the KMS key
- `kms_key_arn`: The Amazon Resource Name (ARN) of the KMS key
- `kms_key_alias_name`: The display name of the KMS key alias
- `kms_key_alias_arn`: The Amazon Resource Name (ARN) of the KMS key alias
- `kms_key_rotation_enabled`: Whether automatic key rotation is enabled
- `kms_key_deletion_window`: The deletion window in days

## Integration Examples

- Use for S3 bucket default encryption.
- Use as the KMS key for Vault storage auto-unseal and Transit engine.
- Use with EBS, RDS, and other AWS services requiring KMS.

## Cleanup

```bash
terraform destroy
```

Note: KMS keys observe the configured deletion window and are not deleted immediately.


