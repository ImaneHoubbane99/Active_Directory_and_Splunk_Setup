# Administer Active Directory Domain Services

This guided project helps prepare you to manage Active Directory Domain Services, including creating and deploying domains, configuring Group Policy Objects, establishing and enforcing passwords, and maintaining the security of Active Directory.

## Setup

The Setup section consists of three main tasks:

- Install Hyper-V
- Create Windows Server Domain Controller Virtual Machine
- Create Windows Server Domain Member Server
  
if you're working in the cloud, you won't need Hyper-V on your local machine. Instead, you'll create virtual machines (VMs) in Azure that simulate your on-premises Windows Server environment.

Steps to create VMs in Azure:

  - Step 1: Sign in to the Azure portal.
  - Step 2: Navigate to Virtual Machines and click Create.
  - Step 3: Select the Windows Server 2022 image from the list of available options. Choose the Windows Server 2022 Datacenter - Evaluation edition if you need a trial version.
  - Step 4: Choose a size for the virtual machine (make sure to select one with at least 16 GB RAM if you need performance similar to a physical host).
  - Step 5: Configure network settings, including setting up a virtual network and public IP address.
  - Step 6: Review and create the VM. This will be your Domain Controller VM.
    
Repeat the process to create a second virtual machine, which will act as your Domain Member Server.

## Step 1: Configure Domain Controller Operations

## Step 2: Configure User Management Operations

## Step 3: Manage Password Policies

## Step 4: Configure Security Settings

