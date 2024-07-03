# Home Office Solution with Microsoft Azure VPN Gateway

This repository provides a step-by-step guide for setting up a Point-to-Site (P2S) VPN using Microsoft Azure VPN Gateway. This solution was designed in response to the COVID-19 pandemic to enable employees to securely access their Azure-hosted environment from their home offices without exposing the environment to the Internet.

## Project Overview

As a Cloud Specialist, you are tasked by the CTO to design a secure solution that allows office workers to continue their tasks from home. By implementing a P2S VPN, employees can establish an encrypted tunnel between their home environment and Azure, ensuring secure access to resources without Internet exposure. This guide outlines the complete setup process, including virtual network creation, virtual machine setup, and VPN gateway configuration.

## Steps

1. **Create Virtual Network**
   - **Resource Group**: `azureresource` (New)
   - **Name**: `VNetBootcamp`
   - **Region**: `East US`
   - **IPv4 Address Space**: `10.0.0.0/16`
   - **Subnet**: Default subnet
   - **Gateway Subnet**: Required for Virtual Network Gateway

2. **Set Up Virtual Machine**
   - **Resource Group**: `azureresource`
   - **Region**: `East US`
   - **Name**: `vpnbootcamp`
   - **Availability Options**: No infrastructure redundancy required
   - **Security Type**: Standard
   - **Image**: Ubuntu Server 20.04 LTS
   - **Authentication Type**: SSH public key (Generate new key pair)
   - **Public Inbound Ports**: None
   - **Networking**: No Public IP (Access only through VPN)

3. **Configure Virtual Network Gateway**
   - **SKU**: `VpnGw1/Generation1`
   - **Virtual Network**: `VNetBootcamp`
   - **Subnet**: Add Gateway subnet
   - **Public IP Address Name**: `vpnip`
   - **Active-Active Mode**: Disabled

4. **Download/Use Certificate ZIP**

5. **Configure Point-to-Site in Virtual Network Gateway**
   - **Address Pool**: `172.16.0.0/24`
   - **Tunnel Type**: IKEv2 and SSTP (SSL)
   - **Authentication Type**: Azure certificate
   - **Root Certificate**:
     - **Name**: `P2SRootCert`
     - **Public Certificate Data**: `rootcertificate.cer` (Contents)
   - **Download VPN Client**

6. **Install Certificates**
   - Click on `clientcert.prx`
   - **Store Location**: Current User
   - **Password**: `azurebootcamp`
   - **Finish**
   - Unzip `vpnbootcamp.zip`
   - Install `VpnClientSetupAmd64.exe` from `WindowsAmd64`
   - Connect to VPN in VPN Settings

7. **Access Virtual Machine**
   - Obtain Private IP of VM
   - Ping VM using GitBash
   - SSH into VM: `ssh azureuser@10.0.0.4 -i tcbvmkey.pem`
   - Install and restart Apache2 web server:
     ```bash
     sudo apt-get update
     sudo apt-get install apache2
     sudo systemctl restart apache2.service
     ```

By following these steps, you will be able to create a secure P2S VPN that allows remote workers to access the Azure environment seamlessly and securely.
