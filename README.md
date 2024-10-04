# Active Directory Project

## Hardware Requirements

- At least 16 GB of RAM
- At least 250 GB of disk space

## Step 1: Design Architecture

For this task, I used **draw.io** to design the network architecture. The setup consists of the following components:

- **Two Servers**:
  - One for **Splunk** (for log aggregation, monitoring, and analysis)
  - One for **Active Directory Domain Services (AD DS)** (to handle directory services)

- **Two Hosts**:
  - A **Target Machine** running **Windows 10** (simulates a client machine)
  - An **Attacker Machine** running **Kali Linux** (simulates potential security threats)

- **Network Components**:
  - **Switch**: To interconnect all the devices in the local network
  - **Router**: To route traffic between networks and the internet
  - **Firewall**: To protect the internal network from external threats and send logs to Splunk 

- **Cloud (Internet)**:
  - Represents the external internet, allowing access to outside resources or cloud services

- **Sysmon**:
  - Installed on key machines (such as the AD server and target machine) to monitor and log critical system events. This helps detect suspicious activities like abnormal process creation, file manipulation, or network traffic.
  - **Splunk Universal Forwarder** is installed on the AD and target machines to send logs and events to the Splunk server for centralized monitoring.

### Additional Recommendations:
- **Network Segmentation**: Consider dividing the network into segments using VLANs to increase security and limit potential attack surfaces.
- **SIEM Integration**: Use Splunk for security monitoring and analysis by integrating all logs from the network (e.g., from Sysmon, firewall, AD logs).
- **Backup and Recovery**: Implement a backup system for critical data (especially for the Active Directory database) to ensure disaster recovery.
- **Encryption**: Ensure communication between the servers and hosts is encrypted to prevent eavesdropping or data theft.

![image](https://github.com/user-attachments/assets/8e98b4ba-0b79-45f3-8c5d-04ba19c967e5)

This architecture provides a robust foundation for both operational Active Directory services and security monitoring, with Splunk and Sysmon enhancing the security and visibility of system activities.


## Step 2: Install and Deploy Virtual Machines

To use Active Directory, a server must install the **Active Directory Domain Services (AD DS)** role, and that server must be promoted to a **Domain Controller**. The Domain Controller will handle authentication and authorization using the **Kerberos** protocol.

### Install VirtualBox:

Download and install **VirtualBox** using the following link:
- [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

### Install the Target VM:

1. Download the **Windows 10 ISO** for the target VM using the following link:
   - [Windows 10 Download](https://www.microsoft.com/en-ca/software-download/windows10)
   
2. Deploy the ISO inside **VirtualBox** by creating a new virtual machine and configuring it to boot from the downloaded ISO image.

3. Follow the installation steps to set up Windows 10 as the target machine for testing.

### Additional Notes:
- Once the target VM is deployed, you can configure it to connect to the Active Directory Domain.
- The target VM should have the **Splunk Universal Forwarder** and **Sysmon** installed to monitor and forward security events to your Splunk server for centralized log management.
- Ensure the network settings in VirtualBox are configured properly to allow the target VM to communicate with other machines in the network.

### Install Attacker VM:

### Install 

## Step3 : Install and configure sysmon and splunk

## Step4 : Configure AD(Active Directory)

## Step5: Generate brute force attack using Kali linux
