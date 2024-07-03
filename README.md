# Home Office Solution with Microsoft Azure VPN Gateway

This repository provides a step-by-step guide for setting up a Point-to-Site (P2S) VPN using Microsoft Azure VPN Gateway. This solution was designed in response to the COVID-19 pandemic to enable employees to securely access their Azure-hosted environment from their home offices without exposing the environment to the Internet.

## Project Overview

As a Cloud Specialist, you are tasked by the CTO to design a secure solution that allows office workers to continue their tasks from home. By implementing a P2S VPN, employees can establish an encrypted tunnel between their home environment and Azure, ensuring secure access to resources without Internet exposure. This guide outlines the complete setup process, including virtual network creation, virtual machine setup, and VPN gateway configuration.
![https://i.imgur.com/EcF0yI2.png](https://i.imgur.com/EcF0yI2.png)
![https://i.imgur.com/lnHewIO.png](https://i.imgur.com/lnHewIO.png)

## Steps

1. **Create Virtual Network**
   - **Resource Group**: `azureresource` (New)
   - **Name**: `VNetBootcamp`
   - **Region**: `East US`
   - **IPv4 Address Space**: `10.0.0.0/16`
   - **Subnet**: Default subnet
   - **Gateway Subnet**: Required for Virtual Network Gateway
        - Under **IP Addresses**, can add a Gateway Subnet by clicking `+ Add a subnet` and select **Subnet Purpose** - **Virtual Network Gateway**
![https://i.imgur.com/FsqVHtl.png](https://i.imgur.com/FsqVHtl.png)
      

2. **Set Up Virtual Machine**
   - **Resource Group**: `azureresource`
   - **Region**: `East US`
   - **Name**: `VM1`
   - **Availability Options**: No infrastructure redundancy required
   - **Security Type**: Standard
   - **Image**: Ubuntu Server 20.04 LTS
   - **Authentication Type**: SSH public key (Generate new key pair)
   - **Public Inbound Ports**: None
   - **Networking**: No Public IP (Access only through VPN)

3. **Configure Virtual Network Gateway**
   - **Name**: VNG1
   - **SKU**: `VpnGw1/Generation1`
   - **Virtual Network**: `VNetBootcamp`
   - **Subnet**: Add Gateway subnet
   - **Public IP Address Name**: `vpnip`
   - **Active-Active Mode**: Disabled
        - Will take about 20 minutes to deploy the resource

4. [**Download Certificate ZIP**](https://github.com/Rashon5/COVID-VPN-Gateway-Solution/blob/main/new-certificates-azure.zip)
   - Not to be used as of yet, contains Client Certificate (clientcert.prx) and Root Certificate (rootcertificate.cer)

5. **Configure Point-to-Site in Virtual Network Gateway**
   - **Address Pool**: `172.16.0.0/24`
   - **Tunnel Type**: IKEv2 and SSTP (SSL)
   - **Authentication Type**: Azure certificate
   - **Root Certificate**:
     - **Name**: `P2SRootCert`
     - **Public Certificate Data**: `rootcertificate.cer` (Contents)
   - **Download VPN Client**
![https://i.imgur.com/vJiPRgD.png](https://i.imgur.com/vJiPRgD.png)

6. **Install Certificates**
   - Click on `clientcert.prx`
   - **Store Location**: Current User
   - **Password**: `azurebootcamp` or whatever you want, won't be used
   - **Finish**

![https://i.imgur.com/mCllUHK.png](https://i.imgur.com/mCllUHK.png)  
![https://i.imgur.com/FYLieiC.png](https://i.imgur.com/FYLieiC.png)
![https://i.imgur.com/D2113Nk.png](https://i.imgur.com/D2113Nk.png)
   
   - Unzip `VNG1.zip`
   - Install `VpnClientSetupAmd64.exe` from `WindowsAmd64`
   - Connect to VPN in VPN Settings
![https://i.imgur.com/s5vUfL2.png](https://i.imgur.com/s5vUfL2.png)

7. **Access Virtual Machine**
   - Obtain Private IP of VM
   - Ping VM using GitBash
   - SSH into VM: `ssh azureuser@10.0.0.4 -i tcbvmkey.pem`
![https://i.imgur.com/lNy4gNQ.png](https://i.imgur.com/lNy4gNQ.png)  
   
   - Install and restart Apache2 web server:
     ```bash
     sudo apt-get update
     sudo apt-get install apache2
     sudo systemctl restart apache2.service
     ```

   - Paste Private IP into web browser and visit it
![https://i.imgur.com/ygTlKfT.png](https://i.imgur.com/ygTlKfT.png)

   - Try yourself! Try disconnecting the VPN and see if the webpage will still load when going to the VM's private IP. Then try again with the VPN connected

## Things I Learned

1. **VPN Configuration**: Understanding the setup and configuration of a Point-to-Site VPN to securely connect remote workers to Azure resources.
2. **Azure Networking**: Gaining insights into Azure virtual network setup, including address spaces and subnets.
3. **Virtual Machine Setup**: Learning the process of creating and configuring a virtual machine in Azure, including SSH key generation and network security settings.
4. **Certificate Management**: Handling certificates for secure VPN connections, including creation, downloading, and installation.
5. **Resource Group Management**: Organizing and managing Azure resources within a resource group for better structure and maintenance.

By following these steps, you will be able to create a secure P2S VPN that allows remote workers to access the Azure environment seamlessly and securely.
