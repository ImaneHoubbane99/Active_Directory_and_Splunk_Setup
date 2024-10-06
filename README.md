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

#### Additional Recommendations:
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

### Install the Target VM: 4GB -> windows10 pro -> install windows only->

1. Download the **Windows 10 ISO** for the target VM using the following link:
   - [Windows 10 Download](https://www.microsoft.com/en-ca/software-download/windows10)
   
2. Deploy the ISO inside **VirtualBox** by creating a new virtual machine and configuring it to boot from the downloaded ISO image.

3. Follow the installation steps to set up Windows 10 as the target machine.

### Install Attacker VM:

1. Download the **Kali** for the attacker VM using the following link:
   - [Kali Download](https://www.kali.org/)

2. Deploy the ISO inside **VirtualBox** by creating a new virtual machine and configuring it to boot from the downloaded ISO image.

#### Additional Recommendations:
- Default credentials are: kali/kali
   
### Install Active Directory server: stantard evaluation/50Gb

1. Download the **Windows server 2022 ISO** for the target VM using the following link:
   - [Windows server 2022 Download](https://www.microsoft.com/en-ca/software-download/windows10)
   
2. Deploy the ISO inside **VirtualBox** by creating a new virtual machine and configuring it to boot from the downloaded ISO image.

3. Follow the installation steps to set up Windows 10 as the target machine.

### Install Splunk server: 100Gb

1. Download the **Ubuntu server** for the target VM using the following link:
   - [Ubuntu server Download](https://www.microsoft.com/en-ca/software-download/windows10)
   
2. Deploy the ISO inside **VirtualBox** by creating a new virtual machine and configuring it to boot from the downloaded ISO image.

3. Follow the installation steps to set up Windows 10 as the target machine.

## Step3 : Install and configure sysmon Splunk instance on splunk server VM

- **To have our VMs in the same network:**

  1. **Create a NAT Interface:**
     - First, create a NAT interface and name it "AD-Project".
     
       ![NAT Interface Creation](https://github.com/user-attachments/assets/c34f5439-aff3-4db0-a17d-dfc054b3f1b9)
  
  2. **Configure VMs to Use NAT Network:**
     - In each VM, select NAT Network and specify the name of the NAT network. In my case, it is "AD-Project".
     
       ![NAT Network Configuration](https://github.com/user-attachments/assets/fad578e7-6ebe-46e2-a57d-2141904c0ae5)

- **Static IP Configuration for Splunk Server:**
  - Ensure consistent network accessibility and simplify security configurations by making the IP address of the Splunk server static.
  - Navigate to the file within `/etc/netplan` and apply the following changes:
  
    ![Netplan Configuration](https://github.com/user-attachments/assets/f20b9831-cda9-45ab-bde9-125952e3d3c3)
  
  - Execute the changes with `sudo netplan apply`:
  
    ![Apply Netplan](https://github.com/user-attachments/assets/277f1438-0152-410a-a66c-e9b4679dff24)

- **Downloading and Installing Splunk:**
  - Download the Splunk instance from [splunk.com](https://splunk.com) and select the `.deb` package, appropriate for Ubuntu systems.
  - Share files between the host and VM. I use USB for sharing, but you can choose other methods like shared folders. Identify the USB drive using `lsblk`, then mount it:
  
    ```bash
    sudo mkdir /home/splunk
    sudo mount /dev/sdxx /home/splunk  # Replace 'sdxx' with your device identifier, e.g., /dev/sdc1
    ```
  
  - Install Splunk on the VM:
  
    ![Splunk Installation](https://github.com/user-attachments/assets/9a6d3dd4-b145-4e48-ba9c-3c028144bbdb)
  
  - Start Splunk by navigating to `/opt/splunk/bin` and executing `./splunk start`. During this process, you will specify the username and password for the Splunk instance.
  
- **Auto-start Splunk on VM Boot:**
  - Configure Splunk to automatically run each time the VM boots up.
  
    ![Splunk Auto-start](https://github.com/user-attachments/assets/ccd864ab-9f20-45b6-932b-697a16c902ca)


## Step4 : Install and configure sysmon sysmon and splunk forwarder on target machine and Active Directory server
  I will perfoem steps for installing sysmon and splunk forwarder on directory server (same steps can be followed for target machine)
   1-change name to ADDS (for other target-PC)
   2-change the ip from dhcp to 192.168.10.7/24 (192.168.10.100/24) 192.168.10.1 dns 8.8.8.8
   we can connect to the splunk using  192.168.10.10:8000

   -install splunk universal forwarder
 on splunk.com install splunk forwarder : check the box and on promise : username: admin and generate password
    receiving indexer is ip of splunk server
    
    -install sysmon:
   


## Step5 : Configure AD(Active Directory)

## Step6: Generate brute force attack using Kali linux
