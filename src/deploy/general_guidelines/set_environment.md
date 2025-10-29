# Setting Environment Variables

This guide explains how to properly configure your environment variables before deploying infrastructure components. The environment setup is crucial for secure communication with Vault and proper Terraform execution.

## Prerequisites

Before setting up your environment, ensure you have:

- Complete the [Vault Server deployment](../../initial_setup/vault/vault_server.md) first
- Obtain the root token from `vault_init_output.txt` generated during Vault server deployment

## Update Global Configuration

First, you need to update the `config/global.yml` file with your Vault server details.

For example:

```yaml
...
vault:
  addr: "https://192.168.1.400:8200"
  cacert: "~/client.pem"
```


## Set Vault Token

Export the Vault token that was generated during the Vault server deployment:

```bash
export VAULT_TOKEN="hvs.your_actual_token_here"
```

### Obtaining the Token

The Vault token can be found in the `vault_init_output.txt` file created during the [Vault Server deployment](../../initial_setup/vault/vault_server.md). This file contains:

- **Root Token**: Used for administrative access to Vault
- **Unseal Keys**: Used to unseal the Vault server

> **Security Warning**: Store the `vault_init_output.txt` file in a secure location as it provides full administrative access to your Vault instance.

## Run Environment Setup Script

Navigate to the terraform directory and source the environment setup script:

```bash
cd terraform
source set_env.sh
```

The `set_env.sh` script performs the following actions:

1. **Loads Vault Configuration**: Reads `VAULT_ADDR` and `VAULT_CACERT` from `config/global.yml`
2. **Verifies Environment**: Checks that all required environment variables are set
3. **Retrieves Terraform Variables**: Fetches secrets from Vault KV store and sets them as Terraform environment variables

### Environment Variables Set

The script automatically sets the following Terraform variables by retrieving them from Vault:

- `TF_VAR_proxmox_endpoint`: Proxmox server endpoint
- `TF_VAR_gateway_address`: Network gateway address
- `TF_VAR_vm_username`: Default VM username
- `TF_VAR_vm_password`: Default VM password
- `TF_VAR_pve_token_username`: Proxmox token username
- `TF_VAR_pve_token_id`: Proxmox token ID
- `TF_VAR_pve_token_secret`: Proxmox token secret
