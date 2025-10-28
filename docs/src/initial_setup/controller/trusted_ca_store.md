# Trusted CA Store Configuration

This guide explains how to configure the controller node to trust Proxmox's self-signed certificate, which is essential for secure Terraform operations.

## Overview

Proxmox VE uses self-signed certificates by default, which causes TLS verification errors when Terraform attempts to connect to the Proxmox API. This configuration adds the Proxmox certificate to the system's trusted CA store, enabling secure communication.

## Prerequisites

- Controller node running Ubuntu Server
- Network access to your Proxmox server
- `openssl` package installed (usually pre-installed on Ubuntu)
- Root or sudo privileges

## Configuration Steps

### Step 1: Set Proxmox Server IP

First, export your Proxmox server IP address as an environment variable:

```bash
export PROXMOX_IP="<PROXMOX_IP>"
```

**Replace `<PROXMOX_IP>` with your actual Proxmox server IP address.**

Example:
```bash
export PROXMOX_IP="192.168.1.100"
```

### Step 2: Download the Proxmox Certificate

Download the Proxmox server's certificate to your controller node:

```bash
echo | \
  openssl s_client -connect ${PROXMOX_IP}:8006 -showcerts 2>/dev/null | \
  openssl x509 -outform PEM > proxmox.crt
```

### Step 3: Add Certificate to System CA Store

Copy the certificate to the system's trusted CA certificates directory:

```bash
sudo cp proxmox.crt /usr/local/share/ca-certificates/proxmox.crt
```

### Step 4: Update Trusted CA Certificates

Update the system's trusted CA certificates:

```bash
sudo update-ca-certificates
```

You should see output similar to:
```
Updating certificates in /etc/ssl/certs...
1 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
```

### Step 5: Verify Certificate Installation

Verify the certificate was successfully added:

```bash
ls -la /usr/local/share/ca-certificates/proxmox.crt
```

The file should exist and be readable.

### Step 6: Clean Up

Remove the temporary certificate file:

```bash
rm proxmox.crt

```

## Verification

To verify that the certificate is properly trusted, you can test the connection:

```bash
curl -I https://${PROXMOX_IP}:8006/api2/json/version
```

This should return a successful HTTP response without certificate errors.

## Alternative Configuration (Less Secure)

If you prefer not to trust the certificate system-wide, you can configure Terraform to skip certificate verification by setting `insecure = true` in your Proxmox provider configuration:


```hcl
provider "proxmox" {
  endpoint  = "your endpoint"
  
  username  = "your username"
  password  = var.user_terraform_password
  insecure  = true
}

```

**⚠️ Security Warning**: Using `insecure = true` bypasses certificate verification and makes your connection vulnerable to man-in-the-middle attacks. The trusted CA store method is recommended for production environments.


## Security Considerations

- The certificate is added to the system-wide trusted CA store, affecting all applications on the controller node
- This is the recommended approach for production environments
- Consider certificate rotation if your Proxmox server's certificate changes
- Monitor for any security updates related to certificate handling
