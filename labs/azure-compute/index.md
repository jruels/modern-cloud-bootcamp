# Azure Compute


Azure provides a compute service that allows creating resizable instances in the cloud. These compute instances are available in many different sizes (A series, D series etc..) and come by default with 2 drives.

We will be creating the following resources:
1. Deploy a web server using a prebuilt image by Bitnami with Nginx installed.
2. Create an Availability Set which will be used in later labs.

## Create an Availability Set

### Availability Sets
Azure offers “Availability Sets” (AS) which are used to provide redundancy to your
environments virtual machines. By creating an AS and adding VMs to it Azure will ensure
VMs in the set are distributed across different physical hosts. These hosts are cabled up to
different switches in redundant racks in a way where any hardware failures will not cause
downtime to machines in the set.


1. Log into the Azure Dashboard
2. Click + Create a resource > In the search box type “Availability Set” and press enter.
3. Click “Availability Set”, click “Create” and and fill in with below information:

```
Name: AS1
Resource group: CR1
Location: West US
```

> NOTE: “Fault domains” places the VMs in different racks and “Update domains” places VMs on different physical hosts.  
 
Click "Create".


## Build Nginx web server

1. Create Web1 VM using Nginx image
2. In the search box type nginx bitnami and select "NGINX Open Source Certified by Bitnami"
3. Go through process of creating VM
  1. Set size to "A1"
  2. Choose password for Authentication type and set password to `AzurePassword1234`
4. Make sure to add Web1 to Availability Set "AS1".

## Allow HTTP through Web1 Security Group

1. In the Azure dashboard Click Web1 VM, under “Settings” click “Networking, and confirm HTTP and HTTPS are allowed from any source. If not follow below steps.


  2. On right side of screen click “Add inbound port rule”, choose HTTP for the service and click “OK”



## Connecting to Nginx website on Web1 VM

1. In the Azure dashboard Click “All Resources” in the blade on the left side > select the Web1 VM and note down the IP address.

3. Open a browser and enter the IP for Web1 and if everything is configured correctly you should see the page load:

