# Azure Compute


Azure provides a compute service that allows creating resizable instances in the cloud. These compute instances are available in many different sizes (A series, D series etc..) and come by default with 2 drives.

We will be creating the following resources:
1. Deploy SQL1 in Web-Subnet1 and configure Web1 Network Security Group for inbound internet access.
2. Deploy SQL1 in Web-Subnet1 and configure Web1 NSG for inbound internet access.
3. For our web server we will be using a prebuilt image by Bitnami with Nginx installed.
4. Add a data disk to SQL1 VM.
5. The image we will be using is Windows Server 2016 with SQL Server 2017 express installed. This image also installs SQL Server Management Studio (SSMS) on the VM, which is used for connecting to the Azure database later.
6. Create an Availability Set which will be used in later labs.

## Lab 7: Creating Azure Windows Compute Instance  
1. Click + Create a resource > In the search box type SQL Server 2017 Express, press enter, and select "SQL Server 2017 on Windows Server 2019"

2. Fill in the fields with the following:


```
Name: SQL
Username: admin
Password: Password
Resource group: CR
Location: West US
```

Click "OK"

3. Set size to "DS1_v2" 

4. Now on the Settings screen leave default for storage account and IP then fill in the remaining fields:


```
Virtual network: VNET
Subnet: Web-Subnet
Public IP address: (new) VM1-ip
Select public inbound ports: No public inbound ports
```

> NOTE 1: NSG “SQL-nsg” is created and applied at the VM NIC level. By default it allows inbound RDP connections from the internet. In later labs we will allow inbound HTTP connections to the Web1 VM.  

5. On the `SQL Server settings` page accept default values.   

6. On the “Summary” page confirm everything looks good and click “Create”.
Deployment will take around 15 minutes, while waiting continue onto Lab 8.

## Lab 8: Create an Availability Set

1. Log into the Azure Dashboard
2. Click + Create a resource > In the search box type “Availability Set” and press enter.
3. Click “Availability Set”, click “Create” and and fill in with below information:

```
Name: AS
Resource group: CR
Location: West US
```

> NOTE: “Fault domains” places the VMs in different racks and “Update domains” places VMs on different physical hosts.  
 
Click "Create".


## Lab 9: Build Nginx web server

1. Create Web1 VM using Nginx image
2. In the search box type nginx bitnami and select "NGINX Open Source Certified by Bitnami"
3. Go through process of creating VM
4. Make sure to add Web1 to Availability Set "AS1".

## Lab 10: Allow HTTP through Web1 Security Group

1. In the Azure dashboard Click Web1 VM, under “Settings” click “Networking, and confirm HTTP and HTTPS are allowed from any source. If not follow below steps.


2. On right side of screen click “Add inbound port rule”, choose HTTP for the service and click “OK”



## Lab 11: Connecting to Nginx website on Web1 VM

1. In the Azure dashboard Click “All Resources” in the blade on the left side > select the Web1 VM and note down the IP address.

3. Open a browser and enter the IP for Web1 and if everything is configured correctly you should see the page load:

## Lab 12: Connecting to SQL VM

1. In the Azure dashboard Click “All Resources” in the blade on the left side, search for SQL and select it.
2. If you are on a Mac you will need to confirm you have the RDP client installed, if not install it from Mac App Store.   
3. On the SQL1 overview page click “Connect” and it will download an RDP connection file.
Open this file and fill in the username and password with what you provided when you created the virtual machine.
4. If you run into problems with the file, just open RDP and manually enter the information.

This allows you to connect to SQL1 from your desktop.


## Lab 13: Adding Data Disk to SQL1 VM

1. In the Azure dashboard Click “All Resources” in the blade on the left side > Select SQL VM and under “Settings” click “Disks”


2. To add a new disk click “Add data disk” and under “Name” click “create disk” then fill it as below:

```
Resource group: CR1
Account type: Premium
Source type: None 
Size: 1023
```

3. After creating the new disk remember to click “Save” at the top of screen.

## Lab 14: Initialize Data Disk in Windows
You have now attached the new disk, but Windows doesn’t know how to use it. We need to initialize and format the disk so Windows can access it.

1. Connect to SQL1 using RDP
2. Open Server Manager and in the left page click “File and Storage Services”
3. The “Disks” section lists the disks available to the VM. In most cases you will see disk 0, disk1, disk 2, disk 3 etc..
Disk 0 is the operating system disk and Disk 1 is the temporary disk.
4. Right click the disk with partition listed as “Unknown” and select “Initialize”, confirm in the pop-up you want to erase the disk and wait for it complete initializing.

Optional Lab: Create a new volume and mount it on the Windows filesystem.


