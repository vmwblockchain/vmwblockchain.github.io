---
layout: single
title: "Horizon Lab Manual"
categories: labs
date: 2018-07-10
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---

## Introduction

We do have a working Horizon environment. You are using it to jump on the Workshop SDDC. This Horizon environement is running on our BU SDDC.
In this Lab we will conect you Student SDDC vCenter to this existing Horizon environment to rollout Desktops. You can then see the new created pool.
Hold in mind. Only one of the Students per SDDC can do this task.

## What is Horizon on VMware Cloud on AWS

VMware Horizon® 7 for VMware Cloud™ on AWS delivers a seamlessly integrated hybrid cloud for virtual desktops and applications. It combines the enterprise capabilities of the VMware Software-Defined Data Center, delivered as a service
on AWS, with the market-leading capabilities of VMware Horizon for a simple, secure, and scalable solution. You can easily extend desktop services to address more use cases such as on-demand capacity, disaster recovery, and cloud co-location without buying additional data center resources.

![1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/1.png)

### Simplify Public and Hybrid Cloud Management

For customers who are already familiar with Horizon 7 or have Horizon 7 deployed
on premises, running Horizon 7 on VMware Cloud lets you leverage a unified
architecture and familiar tools. You can simplify management for Horizon 7
deployments using on-premises infrastructure and VMware Cloud on AWS with
Cloud Pod Architecture (CPA) by linking cloud deployments in different regions,
or by linking on-premises deployments to VMware Cloud on AWS deployments.
This means that you use the same expertise and tools you know from VMware
vSphere® and Horizon 7 for operational consistency, and leverage the rich feature
set and flexibility you expect from Horizon 7

![2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/2.png)

<!-- 
## Create a Cross SDDC VPN

We will be setting up a IPSEC VPN connection between your VPC and the VPC where Horizon Connection Server is already installed.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/Cross-SDDC-VPN-HZ.png)

1. Go back to the **VMware Cloud on AWS** tab.
2. In the main SDDC windows, click on **View Details**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-106-Image-164.png)
3. Then click on the **Network** menu

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-107-Image-165.png) 

In the Management Gateway box, make a note of the Public IP and the Infrastructure Subnet CIDR.
Make also a note of the Compute Gatway box. Public IP and the Subnet you created in the previous LAB.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-107-Image-166.png)![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/Cross-SDDC-VPN-HZ2.png)

Scroll down a little to get to the Management Gateway setting

1. Click the drop down arrow to open the **IPsec VPNs** section
2. Click on **ADD VPN**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-102-Image-158.png)

Fill in the following information

1. Make sure the drop down is opened, if not click it under **Management Gateway**
2. Click on **Add VPN**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-108-Image-161.png)

    Fill in the following information
3. Name this VPN **Student MGW# to Host CGW** (where # is your student number)
4. Enter **54.70.191.234** for the Remote Gateway Public IP
5. Enter  **192.168.20.0/24** for SDDC under remote network
6. Pre-shared key is **VMware1!**
7. Click on **Save**.
8. Do the same for the same task for establishing connection from Studen SDDC compute gateway and Host coumpute gateway.

Fill in the following information

1. Make sure the drop down is opened, if not click it under **Compute Gateway**
2. Click on **Add VPN**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-108-Image-161.png)

    Fill in the following information
3. Name this VPN **Student CGW# to Host CGW** (where # is your student number)
4. Enter **54.70.191.234** for the Remote Gateway Public IP
5. Enter  **192.168.20.0/24** for SDDC under remote network
6. 6. Select **Studen#-LN** and also **cgw-dns-network** in Local Networks
7. Pre-shared key is **VMware1!**
8.  Click on **Save**.
-->

## Configuring SDDC Firewall Rules

If not done already in the previous lab please also create the Firewall rule for the Management Gateway so you can access the vCenter.

### Compute Gateway Firewall Rules

Like the Management NSX Edge Services Gateway. By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow access to your workload VMs which you provision in the VMware Cloud on AWS platform.
<!--  
#### Create Firewall Rule under Compute Gateway 

1. Under **Network** tab, navigate to **Compute Gateway**
2. Expand **Firewall Rules**
3. Click **ADD RULE**

Follow the same process as in the previous step and create Horizon Inbound and Outbound Firewall Rule following these instructions:
Just to hold it easy use the Any Any rule.

1. **Name** - Horizon
2. **Action** - Allow
3. **Source** - Any 
4. **Destination** - Any
5. **Service** - ANY
6. Click **SAVE** button.
-->

<!--  
## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

You can create two types of libraries: local or subscribed libraries.

### Local Libraries

You use a local library to store items in a single vCenter Server instance. You can publish the local library so that users from other vCenter Server systems can subscribe to it. When you publish a content library externally, you can configure a password for authentication.

VM templates and vApp templates are stored as an OVF file format in the content library. You can also upload other file types, such as ISO images, text files, and so on, in a content library.
-->

## Cretea a Logical Network

For our Horizon Lab we prepared several machines like AD, Hoirzon Connections Server, Unified access gateway and the Goldenmaster Image.
The AD, Horizon Connection Server, UAG and Goldenmaster Image will be deployed in a 192.168.20.0/24 subnet. Therefore we need to create this network first.

## Create a Logical Network

1. Once you are logged in to your vCenter Server Click on **Menu**
2. Select **Global Inventory Lists**
3. Click on **Logical Networks** in the left pane
4. Click on the **Add** button
5. Name your New Logical Network **Horizon#-LN** (where # is your student number)
6. Select the **Routed Network** radio button
7. For CIDR Block enter **192.168.20.0/24**
8. Enter **192.168.20.1** for the Default Gateway IP
9. Make sure DHCP is disabled
10. Click **OK** to create your new logical network

## Subscribed Libraries

You subscribe to a published library by creating a subscribed library. You can create the subscribed library in the same vCenter Server instance where the published library is, or in a different vCenter Server system. In the Create Library wizard you have the option to download all the contents of the published library immediately after the subscribed library is created, or to download only metadata for the items from the published library and later to download the full content of only the items you intend to use.

To ensure the contents of a subscribed library are up-to-date, the subscribed library automatically synchronizes to the source published library on regular intervals.

You can also manually synchronize subscribed libraries. You can use the option to download content from the source published library immediately or only when needed to manage your storage space.

Synchronization of a subscribed library that is set with the option to download all the contents of the published library immediately, synchronizes both the item metadata and the item contents. During the synchronisation the library items that are new for the subscribed library are fully downloaded to the storage location of the subscribed library.

Synchronization of a subscribed library that is set with the option to download contents only when needed synchronizes only the metadata for the library items from the published library, and does not download the contents of the items. This saves storage space. If you need to use a library item you need to synchronize that item. After you are done using the item, you can delete the item contents to free space on the storage. For subscribed libraries that are set with the option to download contents only when needed, synchronizing the subscribed library downloads only the metadata of all the items in the source published library, while synchronizing a library item downloads the full content of that item to your storage. If you use a subscribed library, you can only utilize the content, but cannot contribute with content. Only the administrator of the published library can manage the templates and files.

In the subscribed content library you will find the Golden Master Image that you need to use for the deploymend of new desktops with the help of horizon

![Page-19-Image-19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-19-Image-19.png)

1. Click on **Menu**
2. Click on **Content Libraries**

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![Page-20-Image-20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-20.png)

1. In your Content Library window, click the **+** sign to add a new Content Library.
    ![Page-20-Image-21](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-21.png)
2. Name your Content Library **Student#-HorizonGM** where **#** is the number assigned to you
3. (Optional) Enter some notes for your Content Library
4. Click **Next** button

5. Select **Subscribed content library**
6. Under **Subscription URL** enter the following:

    ```link
    https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/6f0bc23f-3157-4fb5-a4c4-2f3f180b8d8d/lib.json
    ```

7. Ensure Download content is set to **when needed**
8. Click **Next**
    ![Page-22-Image-23](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-23.png)
9. Highlight the **WorkloadDatastore** as the storage location
10. Click **Next**
    ![Page-22-Image-24](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-24.png)
11. Click **Finish**. Your content library should take about ~20 minutes to complete syncing.

Now that we have subscribed to the Conten Library we can deploy the Horizon Infrastructre:

1. Click on **Menu**
2. Click on **Content Library**
3. Click on the content library you subscribed to in the previus lab
4. Click on **Templates**
    ![GM-W10-1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/GM-W10-1.png)

## Create your Active Directory VM

1. Right Click on the **VMCWINDC01** and choose **New VM from this Template....**
2. Give it the same the name **VMCWINDC01**
3. As location click on **Workloads**
4. Click on **Next**
5. Select **Compute-ResourcePool** and click **Next**
6. Click **next**
7. Select **WorkloadDatastore** and click **next**
8. Select the network you created in privious LAB **Horizon#-LN**
9. Click **next** and **finish**

## Create Horizon Server VM

1. Right Click on the **HZ-76-WS** and choose **New VM from this Template....**
2. Give it the same the name **HZ-76-WS** where # is put your student ID in
3. As location click on **Workloads**
4. Click on **Next**
5. Select **Compute-ResourcePool** and click **Next**
6. Click **next**
7. Select **WorkloadDatastore** and click **next**
8. Select the network you created in privious LAB **Horizon#-LN**
9. Click **next** and **finish**

## Create your Golden Master Image

With Horizon 7.6 we do have the option to also do Instant Clones. For this lab we prepared two Golden Master Images. The first one is for Instant Clones with the Name **W10-LTBS-1607-IC**, the second one is for Full Clones. You can decide to either go for Full clones or use Instant Clones. We suggest to do instant clones cause it is much faster to rollout this desktops.

1. Right Click on the **W10-LTBS-1607-IC Template** and choose **New VM from this Template....**
2. Give it the same the name **W10-LTBS-#** where # is put your student ID in
3. As location click on **Templates**
4. Click on **Next**
5. Select **Compute-ResourcePool** and click **Next**
6. Click **next**
7. Select **WorkloadDatastore** and click **next**
8. Select the network you created in privious step for 192.168.20.0/24 **Horizon#-LN**
9. Click **next** and **finish**

## Power on the new created VM's

1. Power on the VM **VMCWINDC01**
2. Launch the Web Console
3. Sign in with  **corp\vmcws1** and password **VMware1!**
4. Power on the VM **HZ-76-WS**
5. Launch the Web Console
6. Sign in with  **corp\vmcws1** and password **VMware1!**

Wait about 10 minutes until all services are runnig. In the meantime jump create the UAG VM

## Create UAG VM

1. Go back to your vCenter web Client
2. Right click on the compute ressource pool
    ![customization9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization9.png)
3. click on deploy ovf template
4. ![customization10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization10.png)
5. Select **Local Files**
6. Click **Choose Files**
7. Go to Z://Horizon/ and select euc-unified-access-gateway-3.3.0.0-8539
    ![customization11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization11.png)
8. Click **Open**
9. Click **next**
10. Click **next**
11. Click **next**
12. Select **Single NIC**
13. Click **next**
14. Select Destination Network for all three networks **Horizon#-LN**
15. Following Settings need to be done:
    ![customization12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization12.png)
    ![customization13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization13.png)
    ![customization14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization14.png)
    ![customization15](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization15.png)
16. Click **Finish**
17. Wait until the new created UAG VM is powered on.
18. Open you browser and go to :

    ```link
    https://192.168.20.73:9443
    ```

    ![customization16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization16.png)
19. Click on Import Settings
    ![customization17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization17.png)
20. Browse to ** z://horizon/...
    ![customization18](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization18.png)
21. Click **Import**
22. Login to
    ```link
    https://192.168.20.72/9443
    ```
23. choose the right side **configure manually** and click **select**
24. Click on Edge Services Settings **show**
    ![customization19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization19.png)
25. Tunnel, BLAST,UDP Tunnel Server, HORIZON DESTINATION Server have to be **GREEN**

Now we will request a public IP adress. We will use this public IP adress to access the Horizon infrastructre afterwards. Please go back to VMC console in your browser. Go to the network tab.

1. Scroll down to Compute Gateway and request a new public ip.
2. Please note this public IP.
    ![customization8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization8.png)
3. Click on **NAT**
4. Click on **ADD NAT RULE**
5. Under Description type **Horizon Desktop**
6. Public IP **your requested IP**
7. Service **HTTPS(TCP 443)
8. Public Ports **443**
9. Internal IP **192.168.20.73**
10. Click **SAVE**
11. Check the Firewall Rule you previous created. -> ANY ANY ANY

Now we have to create a Snapshot on the Golden Master Image. Cause Instant Clones are working with snapshots.

1. Right click on **W10-LTBS-1607-IC**
2. Click on **Snapshot**
3. Take a Snapshot
4. type a name **1.0** for example
5. Click **OK**

If decision made to go for Full Clones in the previous step we need to create a windows customization spec for the Full Cones that we will use in Horizon for creating a bunch of VM's and those will be directly placed in the Active Directoy.

## Create a Windows Customization Spec

1. Click on **Menu**
2. Click **Policies and Profiles**
3. Click on **create**
4. Type a Name **Windows**
5. Click **Next**
6. Type Owner name **VMC** Owner organization **VMC** click **Next**
7. Check that **Use the virtual machine name** is selected
8. Do not use a product key **next**
9. Type Password **VMware1!**
10. Click **next**
11. Click **next**
12. Click **next**
13. Select **Windows Server domain** and type **corp.local** under username **vmcws#** where # is please chose your ID and your studen password
14. Click **Finish**

## Create your SDDC vcenter as an enpoint in the existing Horizon environment

That you can create desktops in your SDDC we need to implement your Student SDDC vCenter into the existing Horizon infrastructure.

1. Please open a Browser and navigate to :
    ```link
    https://192.168.20.70/admin
    ```
    ![horizon-server1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server1.png)
2. enter student username and password
3. click **Log In**

    You now can see the Dashboard / manin page of the Horizon Connection Server. This is the place where we will be working the next hour

    ![horizon-server2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server2.png)
4. Click on the left site on Servers:
    ![horizon-server3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server3.png)
5. Click on vCenter Servers **Add**
6. type in server adress "this is the ip adress of your student vcenter" for example 54.72.217.99
7. type in username and password / "cloudadmin@vmc.local and the password from cloudadmin of your student vcenter"
8. you can find your cloudadmin password by going back to the VMC tab in your browser.
    ![horizon-server10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server10.png)
9. if you had filled in the fields: server adress, user name and password click **next**
10. you will get a promt Invalid Certifcate Detected
    ![horizon-server6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server6.png)
11. Click on View Certficate and **Accept**
12. On View Composer settings check **Do not use View Composer** click **Next**
13. Please verfiy that **Enable View Storage Accelerator** is NOT selected
    ![horizon-server8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server8.png)
14. Click **next**
15. Click **Finish**

## Deploy Desktop Pool

Now as we have the vCenter as an Endpoint in Horizon we can start deploying Desktops.

1. On the Horizon Connection Server admin console on the left site you can click on **Desktop Pools**
    ![horizon-pool1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool1.png)
2. Select **Automated Desktop Pool** and click on **Next**
    ![horizon-pool2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool2.png)
3. Select **Dedicated** and click **next**
    ![horizon-pool3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool3.png)
4. you will get a promt "More Information" click **Ignore**
    ![horizon-pool4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool4.png)
5. Select **Full virtual machines** and select **your Student vCenter** with your student vCenter IP
    ![horizon-pool5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool5.png)
6. Type under ID: **Studen-#** Display name **Stunden-#** and select Access group **WS1** under Description **Stundent #**  click **next**
    Note # is your student ID
    ![horizon-pool6.1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool6.1.png)
7. On Desktop Pool Settings under Remote Settings click **Remote machine Power Policy** and select **Always powered on** click **next**
    ![horizon-pool7.1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool7.1.png)
8. scroll down and select **HTML Access** click **next**
    ![horizon-pool8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool8.png)
9. Under " use a naming pattern" enter **studen-#** click **next**
    ![horizon-pool9.1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool9.1.png)
10. Select **Use VMware Virtual SAN**
    ![horizon-pool11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool11.png)
    Now we need to select your GM Image which you converted to a Template
11. Click on **Browse** and select your Golden Master Image for example **W10-LTSB-1**  click **OK**

    If you cant see any template here it might be that you forgot to convert the VM into a Template. As we are working with full clones we have to have a Template. This will change in the future when we will work with Instant Clones.

    ![horizon-pool12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool12.png)
12. Click **browse** on VM folder location and select **Workloads** click **OK**
13. Click **browse** on host and cluster and select the cluster click **OK**
14. Click **browse** next to Ressource pool and select **Compute-ResourcePool**  click **OK**
15. Click **browse** next Storage and select **WorkloadDatastore** click **OK**
16. Verify all fields do have entries and click **next**
    ![horizon-pool13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool13.png)
17. On Advanced Storage Options don't select anything just click **next**
18. Select **Use this customization specification:** and select your customization policy click **netx**
    ![horizon-pool14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool14.png)
19. Check all settings and click **Finish**

## Check if the desktops get created

Go back to your vCenter and see if the cloning starts. This could take up to 5 minutes
