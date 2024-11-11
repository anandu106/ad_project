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
IMAGE
- Save the file and type "sudo netplan apply"
- Type "ip a" and "ping google.com" to test connectivity. 


### Change name and IP address of the Windows 10 VM
- Rename the VM to target-PC.
- Change IP address. 
- Follow the similar steps for the Windows Server. Name the server to ADDC01. Change IP to 192.168.10.7 and reboot. 
  
### Install Splunk Instance on the Ubuntu Server
- Download Splunk Enterprise from here on your host machine. 
- Got to the Ubuntu Server settings in VMware and add a shared folder.
- The path of the folder should be where the Splunk installation file is downloaded on the host machine.
- Go to the Ubuntu Server and type the following command:
sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other -o uid=1000
- If done correctly you should see the following output.
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
- Follow these same steps for the Windows Server.

 ### Download Sysmon for Windows 10 and Windows Server
 - Download Sysmon from <a href="https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon">here.</a>
 - Go <a href="https://github.com/olafhartong/sysmon-modular">here</a> to get the Sysmon configuration
 - Go to sysmonconfig.xml and click on Raw. Save it as a XML document.
 - Extract the Sysmon folder and copy the folder path.
 - Run Powershell as administrator.
 - Change directory to the copied file path.
 - Type ".\Sysmon64 -i ..\sysmonconfig.xml. And install.
 - Go to the following directory and create a file called inputs.conf
 - Add the following information and save.
 - Go to Services. Select SplunkForwarder and select "Local System Account". Restart the service.

### Add Index on Splunk and Enable Forwarding and Receiving
- Go to 192.168.10.10:8000 on your browser and login with the credentials created when installing the Splunk server.
- Create a new index called endpoint.
- Click "Configure receiving"
- Click "New Receiving Port". Provide the port number 9997 and save. 
