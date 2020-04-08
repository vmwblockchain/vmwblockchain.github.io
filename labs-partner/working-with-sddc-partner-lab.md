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

## Introduction

In this lab we are going to start with looking at the basic tasks which you will perform in the VMware Cloud on AWS user interface when you are administering the platform.

## Configuring SDDC Firewall Rules

We will start by allowing inbound internet access to your vCenter managing your SDDC. We have already pre-created the VMware Cloud on AWS SDDC environment for you, in order to save time.

In your browser of choice please login to <https://cloud.vmware.com> click on "Log in" at the top right hand corner of the screen.
    ![cloud.vmwware.com](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/Login1.png)

Select **Login**
    ![cloud.vmware.com (login)](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/Login2.png)

You will be directed to a login page for Cloud Services from VMware. You will need to login with the email address which you provided to the presenter. You will also need to ensure that this email address is associated with a "MyVMware Account" in order for the login to the VMware Cloud Services to work correctly.
    ![Enter credentials](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/Login3.png)

### Firewalls

In VMware Cloud on AWS we have two Edge Firewalls which are protecting the two main networks in the VMware Cloud on AWS SDDC. The **Management Network** and the **Compute Network**. When we first intiate your SDDC environment, the default is for all traffic to both the Management and Compute networks to be denied. In this exercise we will go through the steps required to open up firewall rules so that we can manage the SDDC and not only access compute workloads but allow those compute workloads to communicate with native AWS services.

### Management Firewall Rules

By default, the firewall for the Management Gateway is set to deny all inbound and outbound traffic. In this exercise, you will add a firewall rule to allow vCenter traffic. This will allow you to access the vCenter Server in your SDDC.

Note: This step has to be done by one student only. If you are sharing your SDDC with another student please decide who will be entering the firewall rule. All other tasks are done by both students.

1. From your SDDC, click **View Details**
2. Click **Networking & Security**
3. Go to Security click on **Gateway Firewall** and select **Management Gateway**
4. Click **ADD NEW RULE**
    ![Add New Rule](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/add-mgmt-firewall.jpg)
5. Enter a name for your rule under **Rule Name**, For Example, "vCenter Inbound Rule"
6. Select **Any** for Source
7. Make sure **vCenter** is selected as Destination
8. Select **HTTPS (TCP 443)** from the drop down box for Service
9. Click the **Publish** button on the top right, your rule should look like the below image

![vCenter Rule](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/add_vcenter_firewall.jpg)

Note: In a production environment it is not recommended to open vCenter access from any source but rather from an established VPN connection to a trusted secure network.

### Create Logical Segments

Logical Networks provide network access to workload VMs. VMware Cloud on AWS supports two types of logical network segments, routed and extended.

Routed networks are the default type. Routed networks have connectivity to other logical networks in the same SDDC and to external network services such as compute gateway firewall and NAT. Extended networks require layer 2 Virtual Private Network (L2VPN) which provides a secure communication tunnel between an on-premises network and one in your cloud SDDC.

Your SDDC starts with a single default logical network, sddc-cgw-network-1 (For the purpose of this lab student 1 must delete this network). Each student will use the VMC console to create additional logical networks.

1. From your SDDC, click **View Details**
2. Click **Networking & Security**
3. Go to **Network** and select **Segments**
4. Select **ADD SEGMENTS**
    ![Add New Segment](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/segments.jpg)
5. Name your segment student# (Where # is the student number assigned to you).
6. Select type **Routed**
7. Enter gateway address 192.168.#.1/24 (Where # is the student number assigned to you)
8. Ensure **Enable DHCP** is selected
9. Enter the following DHCP IP range 192.168.#.100-192.168.#200 (Where # is the student number assigned to you)
10. Enter DNS Suffix **corp.local**

Your segment should look similar to the screenshot below...
![Segment](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/add_segment.jpg)

### Compute Gateway Firewall Rules

Like the Management NSX Edge Services Gateway. By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow access to your workload VMs which you provision in the VMware Cloud on AWS platform.

#### Create Compute Firewall rule for Inbound AWS Traffic

We need to add an inbound firewall rule that allows traffic from our AWS VPC into our network segment we created in our VMC environment.

1. Click **ADD NEW RULE**
2. **Name** - Student# AWS Inbound
3. **Source** - Connected VPC Prefixes
4. For **Set Destination** click on "Create New Group"
    ![Create New Group](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/create_new_group.jpg)

    Note: Security Group is a group that categorizes VMs based on VM names, IP addresses, and matching criteria of VM name and security tag.

    Based on the matching Criteria, you can apply a configuration to all the VMs in the security group instead of applying the configuration to the VMs in the SDDC environment individually.
    You can use the security groups when you configure Edge or Distributed Firewalls.

5. Name the group "Group#" (Where # is the student number assigned to you)
6. Under **Member Type** select IP Address
7. Under **Members** enter 192.168.#.0/24 (Where # is the student number assigned to you)
8. Click **Save** (Your group should look similar to the screenshot below)
    ![Group](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/group1.jpg)
9. For **Services** select "Any"
10. Select Publish on top right corner of the screen. (Your Inbound rule should look similar to the screenshot below)
    ![Publish](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/ingress.jpg)

#### Create AWS Outbound Firewall Rule

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1. Click **ADD NEW RULE**
2. **Name** - Student# AWS Outbound
3. **Source** - Select Group#
4. **Destination** - Connected VPC Prefixes
5. **Services** - Any
6. **Action** - Allow
7. Publish Firewall Rule (Your firewall rule should look similar to the screenshot below)
    ![Outbound](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/Outbound.jpg)

#### Create an Outbound Firewall Rule to Allow VM Internet Access

1. Click **ADD NEW RULE**
2. **Name** - Internet Access
3. **Source** - Select Group#
4. **Destination** - Any
5. **Services** - Any
6. **Action** - Allow
7. Publish Firewall Rule

## Log into VMware Cloud on AWS vCenter

We will now login to our VMware Cloud on AWS vCenter instance, now that the required ports have been open on our Management NSX Edge Services Gateway.

Login to your Student # SDDC and click **Open vCenter**

Here you will see the default username and password which has been set for your SDDC vCenter login. You can either view, or copy the password (Please copy the password for the purposes of this lab)

Click **Open Vcenter** from the Default vCenter Credentials view.

This will open a new tab which will take you to the standard vSphere HTML5 client login

Login with **cloudadmin@vmc.local** user name and the password which you copied to the clipboard.

You are now logged in to your VMware Cloud on AWS vCenter Server as **cloudadmin@vmc.local** user.

![vCenter login](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/Screenshot+at+Jul+17+21-26-55.png)

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

1. In the vSphere HTML5 client, click on **Menu**
2. click on **Content Libraries**

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

1. In your Content Library window, click the **+** sign to add a new Content Library.
2. Name your Content Library **Student#** (where # is the number assigned to you)
3. (Optional) Enter some notes for your Content Library
4. Click **Next** button
5. Select **Subscribed content library**
6. Under **Subscription URL** enter the following:

    <https://partner-library.s3-us-west-1.amazonaws.com/ContentLib/lib.json>
7. Do not check **Enable Authentication**
8. Ensure Download content is set to **Immediately**
9. Click **Next**
10. Highlight the **WorkloadDatastore** as the storage location
11. Click **Next**
12. Click **Finish**. Your content library should take about ~20 minutes to complete syncing. Syncing is required prior to proceeding to the next lab. You can take a break during this time prior to proceeding to lab #2 "AWS Integration"

You have now successfully subscribed to a vSphere content library from your VMware Cloud on AWS vCenter instance.
