# Convention Rules

When building Proxmox infrastructure, please follow these naming and addressing conventions

## VM ID Ranges

| Resource Type | ID Range | Description |
|---------------|----------|-------------|
| Template VMs | `1000-1999` | VM templates created with Terraform |
| Regular VMs | `2000-2999` | Production and development VMs |

## IP Address Allocation

| Component | IP Address Range | Description |
|-----------|------------------|-------------|
| Gateway | `192.168.1.1` | Network gateway |
| Proxmox Host | `192.168.1.x` | Proxmox VE server (choose suitable address) |

### Infrastructure Services (192.168.1.2 - 192.168.1.20)
| Service | IP Address | Description |
|---------|------------|-------------|
| Transient IP | `192.168.1.2` | Temporary/transient resources |
| HashiCorp Vault | `192.168.1.3` | Secrets management and configuration |
| *Reserved* | `192.168.1.4-192.168.1.20` | For other DevOps and management services |

### Application VMs (192.168.1.21 - 192.168.1.254)
| Component | IP Address Range | Description |
|-----------|------------------|-------------|
| Other VMs | **192.168.1.21 - 192.168.1.254** *(exclude the address of Proxmox Host)* | Regular VMs and services |

**Notes**: 
- Infrastructure services (1.2-1.20): Reserved for DevOps tools, monitoring, secrets management, and other infrastructure services
- Application VMs (1.21-1.254): Reserved for application workloads, data processing clusters, and other business services
- This allocation provides 19 IPs for infrastructure services and 234 IPs for application VMs
