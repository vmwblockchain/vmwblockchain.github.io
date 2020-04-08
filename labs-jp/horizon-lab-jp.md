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

# Introduction

## What is Horizon on VMware Cloud on AWS

VMware Horizon® 7 for VMware Cloud™ on AWS delivers a seamlessly integrated hybrid cloud for virtual desktops and applications. It combines the enterprise capabilities of the VMware Software-Defined Data Center, delivered as a service
on AWS, with the market-leading capabilities of VMware Horizon for a simple, secure, and scalable solution. You can easily extend desktop services to address more use cases such as on-demand capacity, disaster recovery, and cloud co-location without buying additional data center resources.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/1.png)

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

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/2.png)

## Create a Cross SDDC VPN

We will be setting up a IPSEC VPN connection between your VPC and the VPC where Horizon Connection Server is already installed.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-106-Image-163.png)

1. Go back to the **VMware Cloud on AWS** tab.
2. In the main SDDC windows, click on **View Details**
    (https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-106-Image-164.png)
3. Then click on the **Network** menu

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-107-Image-165.png)

In the Management Gateway box, make a note of the Public IP and the Infrastructure Subnet CIDR

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-107-Image-166.png)

Scroll down a little to get to the Management Gateway setting

1. Click the drop down arrow to open the **IPsec VPNs** section
2. Click on **ADD VPN**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-102-Image-158.png)

Fill in the following information

1. Make sure the drop down is opened, if not click it under **Management Gateway**
2. Click on **Add VPN**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-108-Image-161.png)

    Fill in the following information
3. Name this VPN **Student# to Host** (where # is your student number)
4. Enter **54.70.191.234** for the Remote Gateway Public IP
5. Enter  **Stunen LN network created** for example **192.168.1.0/24** for Stunden 1 SDDC under Remote Networks
6. Pre-shared key is **VMware1!**
7. Click on **Save**.

## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

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

In the subscribed content library you will find the Golden Master Image that you need to use for the deploymend of new desktops with the help of horizon

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-19-Image-19.png)

1. Click on **Menu**
2. Click on **Content Libraries**

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-20.png)

1. In your Content Library window, click the **+** sign to add a new Content Library.
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-21.png)
2. Name your Content Library **Student#-HorizonGM** where **#** is the number assigned to you
3. (Optional) Enter some notes for your Content Library
4. Click **Next** button

    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-21-Image-22.png)
5. Select **Subscribed content library**
6. Under **Subscription URL** enter the following: <https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/ddfe9c01-09ea-4fc2-a03b-91cc7ed5f4b1/lib.json>

    PLEASE NOTE THAT THERE MAY BE AN ISSUE WITH DROPPING/ADDITION OF CHARACTERS FOR THE URL WHEN COPYING AND PASTING FROM THE MANUAL.ASK YOUR INSTRUCTOR IN THE EVENT YOU CANNOT LOCATE IT.

7. Ensure Download content is set to **Immediately**
8. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-23.png)
9. Highlight the **WorkloadDatastore** as the storage location
10. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-24.png)
11. Click **Finish**. Your content library should take about ~20 minutes to complete syncing.

## Import a Windows Customization Spec