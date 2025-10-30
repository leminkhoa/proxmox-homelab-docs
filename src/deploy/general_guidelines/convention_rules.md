# Convention Rules

When building Proxmox infrastructure, please follow these naming and addressing conventions

## VM ID Ranges

| Resource Type | ID Range | Description |
|---------------|----------|-------------|
| Template VMs | `1000-1999` | VM templates created with Terraform |
| Regular VMs | `2000-2999` | Production and development VMs |

## IP Address Allocation


| Type            | Service/Component | IP Address / Range             | Description                                   |
|-----------------|-------------------|---------------------------------|----------------------------------------------|
| Gateway         | Gateway            | `192.168.1.1`                  | Network gateway                              |
| Host            | Proxmox Host       | `192.168.1.x`                  | Proxmox VE server (choose suitable address)  |
| Infrastructure  | Transient IP       | `192.168.1.2`                  | Temporary/transient resources                |
| Infrastructure  | HashiCorp Vault    | `192.168.1.3`                  | Secrets management and configuration         |
| Infrastructure  | Reserved           | `192.168.1.4 - 192.168.1.20`   | Reserved for DevOps and management services  |
| Application VMs | Other VMs          | `192.168.1.21 - 192.168.1.254` | Regular VMs and services (exclude Proxmox Host) |

**Notes**: 
- Infrastructure services **(1.1-1.20)**: Reserved for DevOps tools, monitoring, secrets management, and other infrastructure services
- Application VMs **(1.21-1.254)**: Reserved for application workloads, data processing clusters, and other business services
- This allocation provides **19** IPs for infrastructure services and **234** IPs for application VMs
