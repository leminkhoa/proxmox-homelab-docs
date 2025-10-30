# Infrastructure Configurations

This document provides detailed guidance on configuring infrastructure components for the Proxmox homelab deployment framework. It covers the structure and usage of three essential configuration files that define global settings, VM templates, and deployment specifications.


## Table of Contents

- [Global Configuration (global.yml)](#global-configuration-globalyml)
- [VM Templates Configuration (templates.yml)](#vm-templates-configuration-templatesyml)
- [VM Deployments Configuration (vms.yml)](#vm-deployments-configuration-vmsyml)


## Global Configuration (global.yml)

The `global.yml` file contains system-wide configuration settings that apply across all deployments. This includes network settings, Vault configuration, and other global parameters.

### Configuration Structure

The global configuration follows this structure:

```yaml
transient_ip: <ip_address>
vault:
  addr: "<vault_url>"
  cacert: "<certificate_path>"
```

### Key Components

| Field | Type | Required | Description | Validation Rules |
|-------|------|----------|-------------|------------------|
| `transient_ip` | string | Yes | IP address used for temporary/transient resources during template creation | Must be a valid IP address in infrastructure range (192.168.1.2-192.168.1.20) |
| `vault.addr` | string | Yes | HashiCorp Vault server URL for secrets management | Must be a valid URL and accessible from deployment environment |
| `vault.cacert` | string | No | Path to Vault CA certificate file | Path must exist if specified; |

### Alternative: System Trust Store Configuration

If you prefer not to specify the `cacert` path in the configuration, you can configure your system to trust the Vault CA certificate globally:

1. **Copy the client certificate** from the [Vault image build](../../initial_setup/vault/vault_image.md) downloads folder:
   ```bash
   sudo cp client.pem /usr/local/share/ca-certificates/vault-ca.crt
   ```

2. **Update the system trust store**:
   ```bash
   sudo update-ca-certificates
   ```

   You should see output similar to:
   ```
   Updating certificates in /etc/ssl/certs...
   1 added, 0 removed; done.
   ```

> **Note**: When using system trust store configuration, the `cacert` field can be omitted from the configuration as the system will automatically trust the Vault certificate.

### Example Configuration

```yaml
transient_ip: "192.168.1.2"
vault:
  addr: "https://192.168.1.3:8200"
  cacert: "~/client.pem"
```

## VM Templates Configuration (templates.yml)

The `templates.yml` file defines VM templates that serve as base images for creating deployment instances. Templates are pre-configured VMs with specific software and configurations installed.

### Configuration Structure

Templates are organized by stack type with the following structure:

```yaml
<stack_name>:
  vms:
    - name: "<template_name>"
      id: <template_id>
      ip: "<template_ip>"
      description: "<template_description>"
      tags: ["<tag1>", "<tag2>"]
      vars:
        <key>: "<value>"
```

### Key Components

| Field | Type | Required | Description | Validation Rules |
|-------|------|----------|-------------|------------------|
| `name` | string | Yes | Unique name for the template VM | Must be unique across all templates |
| `id` | integer | Yes | Proxmox VM ID | Must be in range 1000-1999 and unique |
| `ip` | string | No | IP address for the template | If not defined, use a transient ip `192.168.1.2`|
| `description` | string | No | Human-readable description of the template | No validation required |
| `tags` | array | No | List of tags for categorization and filtering | Array of strings, no duplicates |
| `vars` | object | No | Key-value pairs for template-specific variables | Key-value pairs for custom configuration |

### Example Configuration

```yaml
spark_base:
  vms:
    - name: "spark-base"
      id: 1001
      ip: "192.168.1.200"
      description: "Spark Base Template VM"
      tags: ["template", "spark"]
      vars:
        role: "base"
```

## VM Deployments Configuration (vms.yml)

The `vms.yml` file defines actual VM deployments organized by cluster or service type. This includes master/worker configurations, service-specific variables, and template references.

### Configuration Structure

Deployments are organized by cluster type with the following structure:

```yaml
<stack_name>:
  vms:
    <node_type>:
      hosts:
        - name: "<vm_name>"
          id: <vm_id>
          ip: "<vm_ip>"
          tags: ["<tag1>", "<tag2>"]
          vars:
            <key>: "<value>"
      template_id: <template_id>
```

### Key Components

| Field | Type | Required | Description | Validation Rules |
|-------|------|----------|-------------|------------------|
| `name` | string | Yes | Unique name for the VM instance | Must be unique within each cluster |
| `id` | integer | Yes | Proxmox VM ID | Must be in range 2000-2999 and unique |
| `ip` | string | Yes | IP address for the VM | Must be in application range (192.168.1.21-254) and unique |
| `tags` | array | No | List of tags for categorization and Ansible inventory | Array of strings, used for Ansible grouping |
| `vars` | object | No | Service-specific variables and configuration | Key-value pairs passed to Ansible playbooks |
| `template_id` | integer | Yes | ID of the template to use for VM creation | Must reference existing template in templates.yml |

### Example Configuration

```yaml
spark_cluster:
  vms:
    masters:
      hosts:
        - name: "spark-master-1"
          id: 2000
          ip: "192.168.1.101"
          tags: ["vm", "spark_master"]
          vars:
            role: "master"
            spark_master_port: "7077"
            spark_web_ui_port: "8080"
      template_id: 1001

    workers:
      hosts:
        - name: "spark-worker-1"
          id: 2001
          ip: "192.168.1.102"
          tags: ["vm", "spark_worker"]
          vars:
            role: "worker"
            spark_worker_port: "8081"
            spark_worker_web_ui_port: "8081"
      template_id: 1001

minio_cluster:
  vms:
    servers:
      hosts:
        - name: "minio"
          id: 2011
          ip: "192.168.1.106"
          tags: ["vm", "minio"]
          vars:
            role: "server"
            minio_port: "9000"
            minio_console_port: "9001"
      template_id: 1000
```
