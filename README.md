# ACTIVE DIRECTORY HOMELAB

## Summary
Building a home lab with Active Directory environment. In addition, a Splunk instance is also created to ingest events from the Windows Server that has the Active Directory. 
Kali Linux will be used as an attacking machine to generate telemetry. 

## System Requirements
All system/software required for this project will be installed on VMware Workstation. Therefore minimum requirement for the host machine will be:

RAM: 16GB (recommended 32GB)
Disk Space: 500 GB free space. 

The documentation to install Vmware Workstation is provided <a href="https://knowledge.broadcom.com/external/article/344595/downloading-and-installing-vmware-workst.html">here.</a>

## Logical Diagram

## Installing Windows 10, Windows Server, Ubuntu Server and Kali Linux on to Vmware
### Windows 10
Go to the Windows 10 download page <a href="https://www.microsoft.com/en-ca/software-download/windows10">here.</a>

Click Download Tool Now

In Setup, select Create Installation Media, customize options and then select ISO file. 


### Kali Linux
Download Kali Linux VMware image from <a href="https://www.kali.org/get-kali/#kali-virtual-machines">here.</a>


### Windows Server
Download Windows Server 2022 from <a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022">here.</a>


### Ubuntu Server
Download Ubuntu Server file from <a href="https://ubuntu.com/download/server">here.</a>


### In VMware
 
Click New Virtual Machine.

Select Typical.  

Select Installer Disc Image file and add the downloaded Windows 10 iso file. 

Select Microsoft Windows as Guest Operating System.

Name the Virtual Machine and select the location to store the VM files. 

Allocate 50GB size and select Split virtual disk into multiple files.

Open the created VM and start Installing Windows.   

Follow the similar steps for creating Kali Linux, Ubuntu Server and Windows Server VMs.


### Install Splunk Instance on the Ubuntu Server

Download Splunk Enterprise from here on your host machine. 

Download Sysmon for Windows 10 and Windows Server

