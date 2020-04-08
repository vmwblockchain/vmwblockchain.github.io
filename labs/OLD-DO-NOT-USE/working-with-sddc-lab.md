---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-07-17
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introduction

In this lab we are going to start with looking at the basic tasks which you will perform in the VMware Cloud on AWS user interface when you are administering the platform.

## Add a Host to your SDDC

We will start by adding a host to the VMware Cloud on AWS platform. For the purposes of this lab, we have already pre-created the VMware Cloud on AWS SDDC environments for you, in order to save time.

In your chrome browser you will see a bookmark on the bookmarks bar named **VMware Cloud Services**. Please click this bookmark.

You will be directed to a login page for Cloud Services from VMware. You will need to login with the email address which you signed up to the VMware Cloud on AWS Experience day with. You will also need to ensure that this email address is associated with a "MyVMware Account" in order for the login to VMware Cloud Services to work correctly.

You will now be logged into your organization hosting your VMware Cloud on AWS SDDC cluster.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC1.jpg)

1\. On your *Student Workshop #* SDDC, click on the *View Details* button.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC2.jpg)

2\. Click on the *Actions* button in the upper right hand corner of the UI

3\. Click on *Add Hosts*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC3.jpg)

4\. In this student environment, we have restricted the platform to only allow the addition of one host. If this were a customer environment there would not be this restriction

5\. Click the *Add Hosts* button

Your SDDC environment will now go through the process of adding an additional host to the SDDC environment. The process can take up to 10 minutes to complete. In the background automated tasks are provisioning an additional host, adding the host to the SDDC cluster and extending the VSAN cluster to utilize the storage presented by this additional host. All of this is done without human interaction in roughly 10 minutes. You may choose to wait for this action to process or move onto the next exercise and check back later to see your additional host.

Congratulations! You have completed this step. Please move onto configuring your SDDC Firewall rules below.

----------------

## Configuring SDDC Firewall Rules

In VMware Cloud on AWS we have two Edge Gateways which are protecting the two main networks in the VMware Cloud on AWS SDDC. The **Management Network** and the **Compute Network**. When we first initiate your SDDC environment, the default is for all traffic to both the Management and Compute networks to be denied. In this exercise we will go through the steps required to open up firewall rules so that we can manage the SDDC and not only access compute workloads but allow those compute workloads to communicate with native AWS services.

### Management Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC4.jpg)

By default, the firewall for the Management Gateway is set to deny all inbound and outbound traffic. In this exercise, you will add a firewall rule to allow vCenter traffic. This will allow you to access the vCenter Server in your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC5.jpg)

1\. Click *Network* tab

2\. Under Management Gateway expand *Firewall Rules*

3\. Click *Add Rule*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC6.jpg)

4\. Enter a name for your rule under *Rule Name*, For Example, "vCenter-Allow-Any"

5\. Type *Any* for Source

6\. Make sure *vCenter* is selected as Destination

7\. Select *HTTPS (TCP 443)* from the drop down box for Service

8\. Click the *SAVE* button, your rule should look like the below image

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC7.jpg)

### Compute Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC8.jpg)

Like the Management NSX Edge Services Gateway. By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow access to your workload VMs which you provision in the VMware Cloud on AWS platform.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC9.jpg)

Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access

1\. Under *Network* tab, navigate to *Compute Gateway*

2\. Expand *Firewall Rules*

3\. Click *ADD RULE*

#### AWS Inbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC10.jpg)

4\. Name - **AWS Inbound**

5\. Action - **Allow**

6\. Source - **All connected Amazon VPC**

7\. Destination - **192.168.#.0/24** (Where # is your student number)

8\. Service - **ANY**

9\. Click *SAVE* button.

#### Create AWS Outbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC11.jpg)

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1\. Name - **AWS Outbound**

2\. Action - **Allow**

3\. Source - **192.168.#.0/24** (Where # is your student number)

4\. Destination - **All connected Amazon VPC**

5\. Service - **ANY**

6\. Click *SAVE* button.

We have now successfully created rules which will allow us to access our vCenter server over port 443 from any location, in a real world deployment we would change this to only allow communication from a specific IP address range over a private link, once we have a VPN in place. We have also configured our Compute workloads to be able to communicate with any services in our native AWS VPC which our VMware Cloud on AWS environment is connected too.

## Log into VMware Cloud on AWS vCenter

### Settings Tab

We will now login to our VMware Cloud on AWS vCenter instance, now that the required ports have been open on our Management NSX Edge Services Gateway.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC12.jpg)

1\. Login to your Student # SDDC and click on the *Settings* tab. This displays different
connection information for your VMware Cloud on AWS environment like:

2\. Default account information for your vCenter server

3\. URL to vCenter Server

4\. URL to vCenter API Explorer

5\. PowerCLI Connect string to be used if you desire to use PowerCLI to connect to your VMware
Cloud on AWS vCenter Server

6\. vCenter FQDN

7\. vCenter Public IP information

8\. Actual Public IP Address assigned to vCenter

9\. vCenter Private IP

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC13.jpg)

Click on the vSphere Client's HTML5 URL copy button (\#3 above), and login with
cloudadmin@vmc.local User Name and copy the password to your computer's clipboard and
paste it in the Password Field.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC14.jpg)

You have successfully logged in to the vCenter Server in your VMware Cloud on AWS environments as local user cloudadming@vmc.local.

## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment, or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

You can create two types of libraries: local or subscribed libraries.

### Local Libraries

You use a local library to store items in a single vCenter Server instance. You can publish the local library so that users from other vCenter Server systems can subscribe to it. When you publish a content library externally, you can configure a password for authentication.

VM templates and vApp templates are stored as an OVF file format in the content library. You can also upload other file types, such as ISO images, text files, and so on, in a content library.

### Subscribed Libraries

You subscribe to a published library by creating a subscribed library. You can create the subscribed library in the same vCenter Server instance where the published library is, or in a different vCenter Server system. In the Create Library wizard you have the option to download all the contents of the published library immediately after the subscribed library is created, or to download only metadata for the items from the published library and later to download the full content of only the items you intend to use.

To ensure the contents of a subscribed library are up-to-date, the subscribed library automatically synchronizes to the source published library on regular intervals.

You can also manually synchronize subscribed libraries. You can use the option to download content from the source published library immediately or only when needed to manage your storage space.

Synchronization of a subscribed library that is set with the option to download all the contents of the published library immediately, synchronizes both the item metadata and the item contents. During the synchronisation the library items that are new for the subscribed library are fully downloaded to the storage location of the subscribed library.

Synchronization of a subscribed library that is set with the option to download contents only when needed synchronizes only the metadata for the library items from the published library, and does not download the contents of the items. This saves storage space. If you need to use a library item you need to synchronize that item. After you are done using the item, you can delete the item contents to free space on the storage. For subscribed libraries that are set with the option to download contents only when needed, synchronizing the subscribed library downloads only the metadata of all the items in the source published library, while synchronizing a library item downloads the full content of that item to your storage. If you use a subscribed library, you can only utilize the content, but cannot contribute with content. Only the administrator of the published library can manage the templates and files.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC15.jpg)

1\. In the vSphere HTML5 client, click on *Menu*

2\. Click on *Content Libraries*

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC16.jpg)

1\. In your Content Library window, click the *+* sign to add a new Content Library.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC17.jpg)

2\. Name your Content Library *Student#* (where # is the number assigned to you)

3\. (Optional) Enter some notes for your Content Library

4\. Click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC18.jpg)

5\. Select *Subscribed content library*

6\. Under *Subscription URL* enter the following:

```
https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/4aa185b4-3d6e-45b4-90ca-cd3a845d4502/lib.json
```

7\. Click the checkbox for *Enable Authentication*

8\. For the *Password* enter: **VMware1!**

9\. Ensure Download content is set to *Immediately*

10\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC19.jpg)

11\. Highlight the *WorkloadDatastore* as the storage location

12\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC20.jpg)

13\. Click *Finish*. Your content library should take about ~20 minutes to complete syncing.

You have now successfully subscribed to a vSphere content library from your VMware Cloud on AWS vCenter instance.

### Create a Local Content Library

We will now create a local content library for this VMware Cloud on AWS vCenter instance.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC21.jpg)

1\. Click the *+* sign to create a new Content Library

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC22.jpg)

2\. Name your new Content Library: **LocalContentLibrary#** (where # is your student #)

3\. (Optional) Enter some notes about your Content Library

4\. Click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC23.jpg)

5\. Make sure *Local content library* is selected

6\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC24.jpg)

7\. Highlight the *WorkloadDatastore* as the storage location

8\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC25.jpg)

9\. Review your information and click *Finish*

Congratulations, you have created your Local Content Library.

## Create a Logical Network

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC26.jpg)

1\. Once you are logged in to your vCenter Server Click on *Menu*

2\. Select *Global Inventory Lists*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC27.jpg)

3\. Click on *Logical Networks* in the left pane

4\. Click on the *Add* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC28.jpg)

5\. Name your New Logical Network *Student#-LN* (where # is your student number)

6\. Select the *Routed Network* radio button

7\. For CIDR Block enter **192.168.#.0/24** (where # is your student #)

    • If your designated student number is between 1 and 9, your CIDR block should look like this: **192.168.1.0/24** - This example represents student number 1

    • For students 10 thru 20 it should look like this: **192.168.10.0/24** - This example represents student number 10

8\. Enter **192.168.#.1** for the Default Gateway IP - Example: 192.168.1.1

9\. Make sure DHCP is Enabled by clicking on the *checkbox*

10\. Enter **192.168.#.100-192.168.#.200** for IP Range

11\. Type **corp.local** as your DNS Domain Name

12\. Click *OK* to create your new logical network

## Create Linux Customization Spec

When you clone a virtual machine or deploy a virtual machine from a template, you can customize the guest operating system of the virtual machine to change properties such as the computer name, network settings, and license settings.

Customizing guest operating systems can help prevent conflicts that can result if virtual machines with identical settings are deployed, such as conflicts due to duplicate computer names.

You can specify the customization settings by launching the Guest Customization wizard during the cloning or deployment process. Alternatively, you can create customization specifications, which are customization settings stored in the vCenter Server database. During the cloning or deployment process, you can select a customization specification to apply to the new virtual machine.

Use the Customization Specification Manager to manage customization specifications you create with the Guest Customization wizard.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC29.jpg)

1\. In the vCenter HTML5 client click *Menu*

2\. Click *Policies and Profiles*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC30.jpg)

3\. Click on *+ New* to add a new Customization Specification

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC31.jpg)

4\. Give your VM Customization Spec a name, such as *Linux Spec*

5\. Enter a description for it (Optional)

6\. Make sure to select *Linux* in the Guest OS, Target guest OS section

7\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC32.jpg)

8\. Click on the third option, *Enter a name* button

9\. Enter a name for your linux VMs, such as *linux-vm*

10\. Click on the *Append a numeric value* checkbox

11\. Enter **corp.local** for the Domain Name

12\. Click the *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC33.jpg)

13\. Select *US* for Area

14\. Select *Eastern* for Location

15\. Select *Local time*

16\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC34.jpg)

17\. Leave the defaults on the *Network* screen and click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC35.jpg)

18\. Under Primary DNS Server enter **10.46.159.10** and leave the Secondary DNS Server and Tertiary DNS Server blank

19\. Type **corp.local** for DNS Search Paths and click **Add**

20\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC36.jpg)

21\. Review your entries and click *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC37.jpg)

Congratulations! You have successfully created your VM Customization Spec for your Linux VMs. You can also export (Duplicate), Edit, Import, and Export a VM Customization Spec.

## Deploy Virtual Machines

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC38.jpg)

1\. On your Content Libraries *(Menu -> Content Libraries)*, select *Student#* and select the *Templates* tab.

2\. Right click on the *centos01-web* template and select *New VM from This Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC39.jpg)

3\. Name your Virtual Machine *StudentVM#* (where # is your student number)

4\. Expand the location area until you see *Workloads* and highlight it

5\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC40.jpg)

6\. Expand the destination compute resources until you find *compute-ResourcePool*, select it

7\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC41.jpg)

8\. Click the *Next* button on the Review details screen

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC42.jpg)

9\. In the *Select storage* step, highlight *WorkloadDatastore*

10\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC43.jpg)

11\. In the *Select networks* step, click the drop down box to select the Destination Network (you may need to click Browse to see other networks and select your *Student#-LN* network you created previously

12\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC44.jpg)

13\. In the *Ready to complete* section, review and ensure all your selections are correct and click *Finish*

## Convert a Virtual Machine to a Template

In this step you will be cloning your newly created Virtual Machine into a Template for later use in vRealize Automation section.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC45.jpg)

1\. Ensure your VM deployment completed from your previous step

2\. Click on *Menu*

3\. Select *VMs and Templates*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC46.jpg)

4\. Select your newly created VM *Student#* (where # is your student number)

5\. Click on *Template*

6\. Select *Convert to Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC47.jpg)

7\. Click *Yes*  in the Convert to Template prompt

You have completed this module.

Please add comments below if you would like to give feedback on this exercise.
