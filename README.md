# ACTIVE DIRECTORY HOMELAB

## Summary
Building a home lab with Active Directory environment. In addition, a Splunk instance is also created to ingest events from the Windows Server that has the Active Directory. 
Kali Linux will be used as an attacking machine to generate telemetry. 

## System Requirements
All system/software required for this project will be installed on VMware Workstation. Therefore minimum requirement for the host machine will be:

- RAM: 16GB (recommended 32GB)
- Disk Space: 500 GB free space. 
- The documentation to install Vmware Workstation is provided <a href="https://knowledge.broadcom.com/external/article/344595/downloading-and-installing-vmware-workst.html">here.</a>

## Logical Diagram
<img src="https://github.com/anandu106/ad_project/blob/abafa29a4f5920e878e678aeea43647f22a20ee9/Images/logical_diagram.png" width="500">

## Steps
### Installing Windows 10, Windows Server, Ubuntu Server and Kali Linux on to Vmware
#### Windows 10
- Go to the Windows 10 download page <a href="https://www.microsoft.com/en-ca/software-download/windows10">here.</a>

- Click Download Tool Now

- In Setup, select Create Installation Media, customize options and then select ISO file. 


#### Kali Linux
- Download Kali Linux VMware image from <a href="https://www.kali.org/get-kali/#kali-virtual-machines">here.</a>


#### Windows Server
- Download Windows Server 2022 from <a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022">here.</a>


#### Ubuntu Server
- Download Ubuntu Server file from <a href="https://ubuntu.com/download/server">here.</a>


### In VMware
 
- Click New Virtual Machine.

- Select Typical.  

- Select Installer Disc Image file and add the downloaded Windows 10 iso file. 

- Select Microsoft Windows as Guest Operating System.

- Name the Virtual Machine and select the location to store the VM files. 

- Allocate 50GB size and select Split virtual disk into multiple files.
 
- Open the created VM and start Installing Windows.   

- Follow the similar steps for creating Kali Linux, Ubuntu Server and Windows Server VMs.

### Change IP address of the Ubuntu Server
- Type "sudo nano /etc/netplan/50-cloud-init.yaml"
- Edit the file exactly how it is shown below.
<img src="https://github.com/anandu106/ad_project/blob/2f9a0c4817423dfa04b887b45aa350a2a66ccdfb/Images/ubuntu_ip.png" width="1000">

- Save the file and type "sudo netplan apply"
- Type "ip a" and "ping google.com" to test connectivity. 
<img src="https://github.com/anandu106/ad_project/blob/75155166fad37267abdf932523f4bd5293184f05/Images/ubuntu_ip_1.png" width="1000">

### Change name and IP address of the Windows 10 VM
- Rename the VM to target-PC.
<img src="https://github.com/anandu106/ad_project/blob/2b857b99d46dbacd57e0cc3d2295d63a111f34a8/Images/rename_win10.png" width="500">

- Change IP address.
<img src="https://github.com/anandu106/ad_project/blob/2b857b99d46dbacd57e0cc3d2295d63a111f34a8/Images/win10_ip.png" width="500">
<img src="https://github.com/anandu106/ad_project/blob/2b857b99d46dbacd57e0cc3d2295d63a111f34a8/Images/win10_ip_1.png" width="500">
   
- Follow the similar steps for the Windows Server. Name the server to ADDC01. Change IP to 192.168.10.7 and reboot. 
  
### Install Splunk Instance on the Ubuntu Server
- Download Splunk Enterprise from <a href="https://www.splunk.com/en_us/download/splunk-enterprise.html">here</a> on your host machine. 
- Got to the Ubuntu Server settings in VMware and add a shared folder.
- The path of the folder should be where the Splunk installation file is downloaded on the host machine.
<img src="https://github.com/anandu106/ad_project/blob/72eb54a75a7d3382ef9df3e70a2f7d64979dc1d8/Images/Screenshot%202024-11-10%20214854.png" width="500">

- Go to the Ubuntu Server and type the following command:
sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other -o uid=1000

- If done correctly you should see the following output.
<img src="https://github.com/anandu106/ad_project/blob/72eb54a75a7d3382ef9df3e70a2f7d64979dc1d8/Images/Screenshot%202024-11-10%20214925.png" width="500">


- Type "sudo dpkg -i {the name of the downloaded splunk file}" to install.
- After completion go to the directory /opt/splunk
- Type "sudo -u splunk bash" to change to user splunk.
- Type "cd /bin"
- Type "./splunk start" to start installer.
- Choose a username and password when prompted.
- Type "exit" -> "cd bin" -> "sudo ./splunk enable boot-start -user splunk" to boot-up splunk everytime the system reboots.  


### Download Splunk Universal Forwarder for Windows 10 and Windows Server
- Go to the Windows VM and go <a href="https://www.splunk.com/en_us/download/universal-forwarder.html">here</a> to download Splunk Universal Forwarder.
- Click the downloaded file and start the setup.
- Select "An on-premises Splunk Enterprise instance.
- Choose a username and select "Generate random password"
- Skip Deployment Server option. Add the IP of the Splunk server i.e 192.168.10.10 for the receiving indexer. The port is 9997.
- And install.
- Go to the following directory and create a file called inputs.conf
<img src="https://github.com/anandu106/ad_project/blob/72eb54a75a7d3382ef9df3e70a2f7d64979dc1d8/Images/win10_splunk.png" width="500">

- Add the following information and save.
<img src="https://github.com/anandu106/ad_project/blob/72eb54a75a7d3382ef9df3e70a2f7d64979dc1d8/Images/inputs_conf.png" width="500">
 
- Go to Services. Select SplunkForwarder and select "Local System Account". Restart the service.
<img src="https://github.com/anandu106/ad_project/blob/72eb54a75a7d3382ef9df3e70a2f7d64979dc1d8/Images/win10_splunk_1.png" width="500">

- Follow these same steps for the Windows Server.

 ### Download Sysmon for Windows 10 and Windows Server
 - Download Sysmon from <a href="https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon">here.</a>
 - Go <a href="https://github.com/olafhartong/sysmon-modular">here</a> to get the Sysmon configuration
 - Go to sysmonconfig.xml and click on Raw. Save it as a XML document.
 - Extract the Sysmon folder and copy the folder path.
 - Run Powershell as administrator.
 - Change directory to the copied file path.
 - Type ".\Sysmon64 -i ..\sysmonconfig.xml. And install.
 - Follow these same steps for the Windows Server.

### Add Index on Splunk and Enable Forwarding and Receiving
- Go to 192.168.10.10:8000 on your browser and login with the credentials created when installing the Splunk server.
- Create a new index called endpoint.
<img src="https://github.com/anandu106/ad_project/blob/a043e4e8c673c961b0f235150f0d3c2f91f59e8f/Images/splunk_index.png" width="500">

- Click "Configure receiving"
<img src="https://github.com/anandu106/ad_project/blob/a043e4e8c673c961b0f235150f0d3c2f91f59e8f/Images/splunk_forwarding.png" width="500">
 
- Click "New Receiving Port". Provide the port number 9997 and save.


### Configure Active Directory
- Go to the Windows Server VM and click "Add Roles and Features"
- Select "Role-based or feature-based installation"
- Select the ADDC01 server.
- Select Active Directory Domain Services. And install.
- Click "Promote this server to a domain controller"
- Select "Add a new forest"
- For domain name, it must have a top level domain. Example: mylab.local
- Leave the rest of the settings as default. And install.

#### Add Users on Active Directory
- Click on "Active Directory Users and Computers"
- Right click on the domain and select "Organizational Unit"
- Create new User in the newly created OU.
- Go to the Windows 10 VM. Click on Advanced system settings.
- Type the domain name and click OK.
- Change the preferred DNS server for the Windows 10 VM to 192.168.10.7 and reboot.

### Generate Telemetry
- Enable RDP on the Windows target machine.
- Under "Enter the object names to select", type the users created.
IMAGE
- Install crowbar on the Kali machine using sudo apt-get install -y crowbar
- For this project to work add the user passwords to the rockyou.txt file in /usr/share/wordlists/
- 
