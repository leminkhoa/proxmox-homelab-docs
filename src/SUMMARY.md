# Summary

[Home](index.md)
[Prerequisites](./prerequisites.md)
[Proxmox Installation](./proxmox_installation.md)
[Getting Started](./getting_started.md)

# Initial Setup
- [Controller Node Setup](./initial_setup/controller/trusted_ca_store.md)

- [AWS Configuration](./initial_setup/aws/index.md)
  - [S3 Backend](./initial_setup/aws/s3_backend.md)
  - [KMS Backend](./initial_setup/aws/kms_backend.md)

- [Proxmox Setup](./initial_setup/proxmox_setup/index.md)
  - [Users and Roles](./initial_setup/proxmox_setup/users_and_roles.md)
  - [Local Storage](./initial_setup/proxmox_setup/local_storage.md)
  - [Base VM Templates](./initial_setup/proxmox_setup/vm_templates.md)
    - [Ubuntu Server Jammy](./initial_setup/proxmox_setup/base_vm_template_ubuntu_server.md)

- [HashiCorp Vault](./initial_setup/vault/index.md)
  - [Vault Image Builder](./initial_setup/vault/vault_image.md)
  - [Vault Server Deployment](./initial_setup/vault/vault_server.md)
  - [Vault Components Setup](./initial_setup/vault/vault_components.md)

# Deployments
- [Overview](./deploy/index.md)
- [General Guidelines](./deploy/general_guidelines/index.md)
  - [Infrastructure Configurations](./deploy/general_guidelines/build_config.md)
  - [Convention rules](./deploy/general_guidelines/convention_rules.md)
  - [Setting environment variables](./deploy/general_guidelines/set_environment.md)

- [Stacks](./deploy/stacks/index.md)
  - [Spark](./deploy/stacks/spark/index.md)
  - [Kubernetes](./deploy/stacks/k8s/index.md)

# References
- [Deployment modules](./references/deployment_modules.md)
