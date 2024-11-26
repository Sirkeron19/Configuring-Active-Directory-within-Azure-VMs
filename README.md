# Configuring Active Directory within Azure VMs

## Overview
This project demonstrates how to deploy and configure an Active Directory (AD) environment within Azure Virtual Machines (VMs). It covers setting up domain controllers, DNS, networking, and integration with Azure Active Directory (Azure AD) services.

## Prerequisites
- An active Azure subscription
- Basic knowledge of Azure and Active Directory
- PowerShell / Azure CLI tools installed

## Steps

### Step 1: Deploy Azure Virtual Machines
1. Create two Azure VMs to host Active Directory Domain Services.
2. Configure the VMs to communicate with each other over a Virtual Network.

### Step 2: Install Active Directory Domain Services
1. Install AD DS role on both VMs.
2. Promote the first VM to be the primary domain controller.

### Step 3: Network Configuration
1. Set up DNS and other network configurations to ensure proper communication between VMs.
2. Configure Network Security Groups (NSGs).

### Step 4: Optional Azure AD Integration
1. Set up Azure AD Join for hybrid environments.
2. Use Azure AD Connect to synchronize identities.

## Resources
- [Azure Active Directory Documentation](https://learn.microsoft.com/en-us/azure/active-directory/)
- [Active Directory Installation Guide](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/install/install-active-directory-domain-services)

## Scripts
- [Install-ADDS.ps1](scripts/Install-ADDS.ps1): PowerShell script for installing Active Directory Domain Services.
- [deploy-vm.sh](scripts/deploy-vm.sh): Azure CLI script to deploy VMs with necessary configurations.

## Architecture Diagrams
- See `diagrams/azure_ad_architecture.png` for a high-level architecture of the solution.
- # PowerShell Script to Install Active Directory Domain Services
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

# Promote the server to a domain controller
$DomainName = "contoso.com"
$SafeModePassword = "SecurePassword123"
Install-ADDSDomainController -DomainName $DomainName -InstallDNS -Credential (Get-Credential) -SafeModeAdministratorPassword (ConvertTo-SecureString $SafeModePassword -AsPlainText -Force)
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2021-07-01",
      "name": "ADVM1",
      "location": "[resourceGroup().location]",
      "properties": {
        "hardwareProfile": {
          "vmSize": "Standard_DS2_v2"
        },
        "storageProfile": {
          "osDisk": {
            "createOption": "FromImage",
            "managedDisk": {
              "storageAccountType": "Standard_LRS"
            }
          }
        },
        "osProfile": {
          "computerName": "ADVM1",
          "adminUsername": "azureadmin",
          "adminPassword": "Password123!"
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', 'advm1-nic')]"
            }
          ]
        }
      }
    }
  ]
}
git init
git add .
git commit -m "Initial commit - Azure AD Configuration project"
git remote add origin https://github.com/yourusername/azure-ad-vm-config.git
git push -u origin main

Additional Considerations:
You can enhance the project by automating the entire environment setup using Azure DevOps pipelines, GitHub Actions, or Terraform.
Consider adding a section on backup and disaster recovery for the Active Directory setup.
You may also add security best practices for securing the AD environment.
This should help you get started on a comprehensive GitHub project focused on configuring Active Directory in Azure VMs.
