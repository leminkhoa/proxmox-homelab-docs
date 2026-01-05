# Trusted CA Store Configuration

This guide explains how to configure the controller node to trust Proxmox's self-signed certificate, which is essential for secure Terraform operations.

## Overview

By default, Proxmox VE issues self-signed certificates for its API (port `8006`). While functional, these certificates are untrusted by standard TLS libraries, causing Terraform providers to reject connections unless security is explicitly bypassed.

This guide outlines the professional standard for resolving these errors: **importing the Proxmox public key into your controller's local Trust Store**. This ensures secure, encrypted communication without compromising integrity by disabling SSL verification.

## Prerequisites

- **Controller Node**: Ubuntu/Debian-based Linux or macOS.
- **Network Connectivity**: Ability to reach the Proxmox API via port `8006`
- **Privileges**: `sudo` access on Linux or Administrator privileges on macOS.
- **Dependencies**: `openssl` (for certificate extraction) and `curl` (for verification).

## Configuration Steps

### Step 1: Extract the Public Certificate

Rather than manually exporting files from the Proxmox GUI, use `openssl` to pull the public certificate directly from the API endpoint.

Define your server address:
```bash
export PROXMOX_IP="192.168.1.100" # Replace with your node's IP
```

Fetch the PEM-encoded certificate:
```bash
echo | openssl s_client \
  -connect ${PROXMOX_IP}:8006 \
  -showcerts 2>/dev/null \
  | openssl x509 -outform PEM > proxmox-ca.crt
```

### Step 2: Update the Local Trust Store
The process for making the system recognize this certificate varies by opering system.

#### For Linux (Ubuntu/Debian)

1. Move the file to the local CA directory
```bash
sudo cp proxmox-ca.crt /usr/local/share/ca-certificates/proxmox-ca.crt
```
2. Rebuild the trust bundle
```bash
sudo update-ca-certificates
```
*Note: Upon success, the output should indicate 1 added.*

#### For macOS

macOS manages trust via the **System Keychain**. Add the cert to macOS trust store:

1. Open Keychain Access
   - Go to **Applications** → **Utilities** → **Keychain Access**

2. Add the certificate to System keychain
   - Select **System** keychain from the left sidebar
   - Drag `proxmox-ca.crt` into the System keychain, or use **File** → **Import Items** and select the certificate file

3. Configure trust settings
   - Double-click the certificate in the System keychain
   - Expand the **Trust** section
   - Set **"When using this certificate"** → **Always Trust**
   - Close the certificate window and enter your password when prompted

**Alternative: Command Line Method**

If you prefer using the command line, you can add the certificate directly:
```bash
sudo security add-trusted-cert \
  -d \
  -r trustRoot \
  -k /Library/Keychains/System.keychain \
  proxmox-ca.crt
```

After adding via command line, you may still need to configure trust settings using the GUI method above. 


### Step 3: Verify Certificate Installation

Verify the certificate was successfully added:

**For Linux/Ubuntu:**
```bash
ls -la /usr/local/share/ca-certificates/proxmox-ca.crt
```

The file should exist and be readable.

**For macOS:**
```bash
security find-certificate -c "pve" /Library/Keychains/System.keychain
```

This should display certificate information if it was added successfully.


### Step 4: Clean Up

Remove the temporary certificate file:

```bash
rm proxmox-ca.crt
```

## Verification

To verify that the certificate is properly trusted, you can test the connection:

```bash
curl -I https://${PROXMOX_IP}:8006/api2/json/version
```

This should return a successful HTTP response without certificate errors.

## Terraform Integration

With the certificate trusted at the OS level, your provider block remains clean and secure. The `insecure` flag should now be set to `false` (or removed entirely).


```hcl
provider "proxmox" {
  endpoint  = "your endpoint"
  
  username  = "your username"
  password  = var.user_terraform_password
  insecure  = false
}

```

**⚠️ Security Warning**: Using `insecure = true` bypasses certificate verification and makes your connection vulnerable to man-in-the-middle attacks. The trusted CA store method is recommended for production environments.


## Security Considerations
- The certificate is added to the system-wide trusted CA store, affecting all applications on the controller node
- This is the recommended approach for production environments
- Consider certificate rotation if your Proxmox server's certificate changes
- Monitor for any security updates related to certificate handling
