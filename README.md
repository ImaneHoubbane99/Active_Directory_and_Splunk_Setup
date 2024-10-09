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
  I will perform steps for installing Sysmon and Splunk Forwarder on the directory server (the same steps can be followed for the target machine):

1. **Change hostname to ADDS:**
   - For other targets, use "target-PC."

2. **Set a static IP address:**
   - Change the IP from DHCP to `192.168.10.7/24`. For target device, use `192.168.10.100/24`.
   - Set DNS to `8.8.8.8`.
   - Connect to the Splunk server using `192.168.10.10:8000`.

   ![Network Configuration](https://github.com/user-attachments/assets/145410dd-ec0a-4b1d-86e9-85e32a7db066)

3. **Install Splunk Universal Forwarder:**
   - On [splunk.com](https://splunk.com), install the Splunk Forwarder. Check the box for on-premise installation.
   - Username: `admin` and generate a password.
   - Set the receiving indexer to the IP of the Splunk server.

   ![Splunk Forwarder Installation](https://github.com/user-attachments/assets/19e0290a-da52-429e-8562-1ded63b1083e)

4. **Install Sysmon:**
   - Install Sysmon by visiting the [Microsoft Sysinternals download page](https://learn.microsoft.com/fr-fr/sysinternals/downloads/sysmon).
   - Download the Sysmon configuration file from Olaf Hartong's repository: [sysmonconfig.xml](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml).
   - Extract Sysmon to a chosen path.
   - Run PowerShell as an administrator and navigate to the directory where Sysmon was extracted.
   - Execute: `Sysmon -i sysmonconfig.xml`.

   ![Sysmon Installation](https://github.com/user-attachments/assets/f100b437-d567-4e5a-b55a-66e07588f242)

5. **Configure Splunk Forwarder:**
   - Customize the default `inputs.conf` file located at `C:\Program Files\SplunkUniversalForwarder\etc\system\local`.
   - Copy and paste the necessary configuration into `inputs.conf`.
   - To grant the Splunk Universal Forwarder the necessary permissions to retrieve data from the host, you'll need to change the Log On option for the Splunk Forwarder service to use an 
     account that has sufficient permissions.
   - Restart the Splunk Universal Forwarder to apply changes.

   ![Forwarder Configuration](https://github.com/user-attachments/assets/a5a77c16-db9d-492c-86b2-de7e03f4b0c4)

6. **Create an index in Splunk called "endpoint":**

   ![Create Index](https://github.com/user-attachments/assets/b2a6c66a-69a0-4612-9db7-2febdaab7cbf)

7. **Enable Splunk server to receive data:**
   - Navigate to `Settings > Forwarding and Receiving > Configure Receiving > New Receiving Port`.
   - Set the receiving port to `9997`.

   ![Configure Receiving](https://github.com/user-attachments/assets/78d781c9-902b-4a88-b36b-a2b08a6f2bf9)
   
At the end, if you install Sysmon and the Splunk Forwarder on the target VM, you will see two hosts in the Splunk search.
![image](https://github.com/user-attachments/assets/0ee35868-f823-4113-b6e9-2593e6cabbcb)

## Step5 : Install and configure Windows server : Active Directory (AD) and promote domain controller && configure target machine to join domain

### Install Active Directory Domain Services (ADDS)
- Go to **Server Manager** > **Manage** > **Add Roles and Features**
- Select **Role-based or Feature-based installation**
- Select **ADDS** and click **Install**

   ![ADDS Installation](https://github.com/user-attachments/assets/7083cf2c-b316-4be6-9167-a49df3a6e68b)

### Promote Server to Domain Controller
- After installation, select **Promote this server to a domain controller**
- Add a new forest and name it `mydomain.project`

   ![New Forest](https://github.com/user-attachments/assets/99277b9f-99d6-4cb4-adc7-e41e0ae73576)

- Enter the password: 

   ![Password](https://github.com/user-attachments/assets/3f0014e2-78f9-49c8-aadd-cd1abb6139ea)

- Complete the installation.

   ![Installation Process](https://github.com/user-attachments/assets/7aef4b5d-f467-4031-b37f-5b77556b43c4)

### Create Users and Organizational Units
- Open **Server Manager** > **Tools** > **Active Directory Users and Computers**

   ![AD Users](https://github.com/user-attachments/assets/8681b5f8-e387-42d4-b2ab-9e85e9f19964)

- Create Organizational Units: **IT** and **HR**

   ![Create OU](https://github.com/user-attachments/assets/dc4dc56e-3f6c-4592-94d4-08527eaf5236)

- Create users inside **IT** and **HR** organizational units:
   - **IT**: 

     ![User IT](https://github.com/user-attachments/assets/c07ed9ff-ba9e-4255-b5e8-a36a6a21ed1a)

   - **HR**:

     ![User HR](https://github.com/user-attachments/assets/afa116d9-c8a2-427c-8248-7ad798279c84)

### Join Target Machine to the Domain
- Change DNS IP from `8.8.8.8` to your domain controller’s IP (`192.168.10.7`) to resolve the domain name. Verify by using `ipconfig /all`

   ![DNS Configuration](https://github.com/user-attachments/assets/b3edea04-5c90-451f-a4cf-f7c32a59eac9)

   ![Verify DNS](https://github.com/user-attachments/assets/49b99792-7c3e-4fba-b2ec-80dcb4767761)

- On the target machine, go to **PC** > **Properties** > **Advanced System Settings** > **Computer Name** > **Change** > **Domain**
- Enter `mydomain.project` and use an Administrator account with the proper permissions to join the domain.

   ![Join Domain](https://github.com/user-attachments/assets/7955e2ad-df6d-458e-90aa-9f560d69bcca)

### Authenticate Using a User Account
- Log in using another user account: 

   ![User Authentication](https://github.com/user-attachments/assets/1456f9ac-4ca8-4c63-bd0f-b05b8fea5459)

## Step6: Generate brute force attack using Kali linux
- change ip address to 192.168.10.250/24 192.168.10.1 8.8.8.8 disconnect ad connect to take the change
![image](https://github.com/user-attachments/assets/8752f917-1740-401e-921a-46cbb7817524)

- upgrade repo : sudo apt-get update & sudo apt-get upgrade
- create a new repo : mkdir ad-project
- install crowbar:  sudo apt-get install -y crowbar
- sudo gunzip /usr/sharewordlists/rockyou
- cp rockyou ad-project
- using just first 20 passwords : head -n 20 rockyou.txt > password.txt and at the end the correct passwords
  ![image](https://github.com/user-attachments/assets/e33b5931-d2b3-4fd8-982d-d699660efb99)

- allow remote connection for both users on target machine
- Brute force attack : crowbar -u user(tsmith) -b rdp -C password.txt -s 192.168.10.100/32
- view telemetry via splunk : index=endpoint tsmith go to eventcode and multiple with 4625 (failed log on)
















