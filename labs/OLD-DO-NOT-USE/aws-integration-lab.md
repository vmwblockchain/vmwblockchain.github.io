---
layout: single
title: "AWS Integration Lab Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---

# Introduction

One of the most compelling reasons to adopt VMware Cloud on AWS is to integrate your existing systems which sit in your VMware cloud environment, with application platforms which reside in your AWS Virtual Private Cloud (VPC) environment. The intergration which VMware and AWS have created allows for these services to communicate, for free, across a private network address space for services such as EC2 instances, which connect into subnets within a native AWS VPC, or with platform services which have the ability to connect to a VPC Endpoint, such as S3 Storage.

## Understanding Integrations with AWS Services

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-1.jpg)

As the above diagram illustrates, the VMware stack not only sits next to the AWS services, but is tightly integrated with these services. This introduces a new way of thinking about how to design and leverage AWS services with your VMware SDDC. Some integrations our customers are using include:

• VMware front-end and RDS backend

• VMware back-end and EC2 front-end

• AWS Application Load Balancer (ELBv2) with VMware front-end (pointing to private IPs)

• Lambda, Simple Queueing Service (SQS), Simple Notification Service (SNS), S3, Route53, and Cognito

• AWS Lex, and Alexa with the VMware Cloud APIs

These are only a few of the integrations we've seen. There are many different services that can be integrated into your environment.
In this exercise we'll be exploring integrations with both AWS Simple Storage Service (S3) and AWS Relational Database Service (RDS).

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs/working-with-sddc-lab/) concerning Content Library creation, Network creation, and Firewall Rule creation.

### How are these integrations possible?

In addition to sitting within the AWS Infrastructure, there is an Elastic Network Interface (ENI) connecting VMware Cloud on AWS and the customer's Virtual Private Cloud (VPC), providing a high-bandwidth, low latency connection between the VPC and the SDDC. This is where the traffic flows between the two technologies (VMware and AWS). There are no EGRESS charges across the ENI within the same Availability Zone and there are firewalls on both ends of this connection for security purposes.

### How is traffiffic secured across the ENI?

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-2.jpg)

From the VMware side (see image below), the ENI comes into the SDDC at the Compute Gateway (NSX Edge). This means, on this end of the technology we allow and disallow traffic from the ENI with NSX Firewall rules. By default, no ENI traffic can enter the SDDC. Think of this as a security gate blocking traffic to and from AWS Services on the ENI until the rules are modified.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-3.jpg)

On the AWS Services side (see image below), Security Groups are utilized. For those who are not familiar with Security Groups, they act as a virtual firewall for different services (VPCs, Databases, EC2 Instances, etc). This should be configured to deny traffic to and from the VMware SDDC unless otherwise configured.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-4.jpg)

In this exercise, everything has been configured on the AWS side for you. You will however walk through how to open AWS traffic to come in and out of your VMware Cloud on AWS SDDC.

### Compute Gateway Firewall Rules for Native AWS Services

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-5.jpg)

1\. In the VMware Cloud on AWS portal click the "Networking & Security" tab

2\. Click "Groups" in the left pane

3\. Click "ADD GROUP"

#### Name Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-6.jpg)

1\. Type "PhotoAppVM" for the Name

2\. Leave "Virtual Machine" select for Member Type

3\. Click "Set VMs" under Members

#### Select VMs - Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-7.jpg)

1\. Click to select "Webserver01"

2\. Click "SAVE"

#### Save Group - Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-8.jpg)

1\. Click "SAVE"

### Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-9.jpg)

1\. Click "Networking & Security" tab in your VMware Cloud on AWS Portal

2\. Click "Gateway Firewall" in the left pane

3\. Click and select "Compute Gateway"

4\. Click "ADD NEW RULE"

#### Add New Rule - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-10.jpg)

1\. Name your new rule "AWS Inbound"

2\. Click on "Set Source"

#### Select Source - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-11.jpg)

1\. Click to select "Connected VPC Prefifixes"

2\. Click "SAVE"

#### Set Destination - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-12.jpg)

1\. Click on "Set Destination"

#### Select Destination - AWS Inbound (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-13.jpg)

1\. Click to select "PhotoAppVM"

2\. Click "SAVE"

#### Set Service - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-14.jpg)

1\. Click on "Set Service"

#### Set Service - AWS Inbound (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-15.jpg)

1\. Click to select "Any"

2\. Click "SAVE"

#### Publish - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-16.jpg)

1\. Click on "PUBLISH"

#### Add New Rule - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-17.jpg)

1\. Click "ADD NEW RULE"

2\. Name your new rule "AWS Outbound"

3\. Click on "Set Source"

#### Select Source - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-18.jpg)

1\. Click to Select "PhotoAppVM"

2\. Click "SAVE"

#### Set Destination - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-19.jpg)

1\. Click on "Set Destination"

#### Select Destination - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-20.jpg)

1\. Click to select "Connected VPC Prefifixes"

2\. Click "SAVE"

#### Set Service - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-21.jpg)

1\. Click on "Set Service"

#### Set Service - AWS Outbound (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-22.jpg)

1\. Under "Select Services" type "3306"

2\. Select "MySQL" checkbox

3\. Click "SAVE"

#### Publish - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-23.jpg)

1\. Click "PUBLISH"

### Add New Rule - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-24.jpg)

1\. Click on "ADD NEW RULE"

#### Add New Rule - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-25.jpg)

1\. Type "Public In" for Name

2\. Click on "Set Source"

#### Select Source - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-26.jpg)

1\. Click to select "Any"

2\. Click "SAVE"

#### Set Destination - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-27.jpg)

1\. Click on "Set Destination"

#### Select Destination - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-28.jpg)

1\. Click to select "PhotoAppVM"

2\. Click "SAVE"

#### Set Service - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-29.jpg)

1\. Click "Set Service"

#### Set Service - Public In (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-30.jpg)

1\. Type "HTTP 80" under "Select Services"

2\.Click to Select "HTTP"

3\. Click "SAVE"

#### Publish - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-31.jpg)

1. Click "PUBLISH"

## Integration with AWS Simple Storage Service (S3)

### AWS S3 Backed vSphere Content Library

vSphere Content Libraries enable customers to take advantage of different types of storage backing than just vSphere Datastores. The primary requirement is that the content endpoint is accessible over HTTP(s), which means that a number of solutions can be used from a simple web server like Nginx to an advanced distributed object store like AWS's Simple Storage Service (S3).

The workflow to create a 3rd Party vSphere Content Library on S3 is as follows:

1\. Upload and organize the content on S3

2\. Run a python script to index and generate the Content Library metadata

3\. With the ability to remotely index and generate the Content Library metadata files, you do
not have to keep a local copy of all your content. All changes can be made directly on the S3 bucket and then simply re-run the script to generate the updated metadata which can even be scheduled as a simple cron job.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-1.jpg)

The python script referenced which can be downloaded [here](https://code.vmware.com/samples/4388/automating-the-creation-of-3rd-party-content-library-directly-on-amazon-s3) can now index content both locally as well as in a remote S3 bucket.

In case you did not know, S3 usage (ingress/egress) from a customers SDDC is 100% free for VMware Cloud on AWS customers by simply using a linked S3 Endpoint. This means you can take advantage of S3 to store your templates, ISOs and other static files, which can also be shared by other SDDCs. This means you are not consuming any of your primary storage for static content and can be used for what it was meant, for your workloads.

For the purpose of this exercise, and in the interest of time, the contents of this exercise have been uploaded to an existing S3 bucket in AWS for you. Let's create the S3 backed Content Library!

#### Add S3 Backed Content Library

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-2.jpg)

1\. On your VMware Cloud on AWS vCenter window click on *Menu*

2\. Click on *Content Libraries*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-3.jpg)

3\. In your Content Library window, click the *+* sign to add a new Content Library.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-4.jpg)

4\. Name your Content Library *S3 Content Library* or any other name of your choosing

5\. (Optional) Enter some notes for your Content Library

6\. Click *NEXT* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-5.jpg)

7\. Select *Subscribed content library*

8\. Under *Subscription URL* enter the following: **http://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json**

9\. Make sure Download content is set to *immediately*

10\. Click *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-6.jpg)

11\. On the *Unable to very identity of the subscription host* notification click *YES*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-7.jpg)

12\. Highlight the *WorkloadDatastore* as the storage location

13\. Click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-8.jpg)

14\. Click *Finish* button. Your content library should take about 20+ minutes to complete syncing.

It may take a few minutes for your Content to sync, periodically check your content library to ensure you see the content.

Congratulations, you have completed this exercise.

## AWS Relational Database Service (RDS) Integration

Amazon RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost- efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching and backups. It frees you to focus on your applications so you can give them the fast performance, high availability, security and compatibility they need.

In this exercise, you will be able to integrate a VMware Cloud on AWS virtual machine to work in conjunction with a relational database running in Amazon Web Services (AWS) that has been previously setup on your behalf.

### Make Note of Webserver01 IP Address

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

You will be using the VM created in the previous module in order to complete this exercise.

1\. In your vCenter interface for VMware Cloud on AWS, find your "Webserver01" VM you deployed, and ensure it has been assigned an IP address as shown in the graphic.

### Assign Public IP

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

1\. Go back your VMware Cloud on AWS portal and click on the "Networking & Security" tab in order to request a Public IP address

2\. Click "Public IPs" in the left pane

3\. Click on "REQUEST NEW IP"

4\. In the notes area type "PhotoApp IP"

5\. Click "SAVE"

### Note New Public IP

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

Take note of your newly created Public IP.

### Create a NAT Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

1\. Click "NAT" in the left pane

2\. Click "ADD NAT RULE"

3\. Type "PhotoApp NAT" for Name

4\. Ensure the Public IP you requested in the previous step appears under Public IP

5\. Leave "All Traffic" (no change)

6\. Type the IP address of your "Webserver01" VM you noted at the beginning of this exercise

7\. Click "SAVE"

### AWS Relational Database Service (RDS) Integration

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS5.jpg)

On your browser, open a new tab and go to: https://console.aws.amazon.com

1\. Account ID or alias - Please refer to the information on the card provided to you for Account ID information

2\. IAM user name - elw_user

3\. Password - VMConAWSelw1!

4\. Click "Sign In"

Please note you might get either of the 2 sign on screens above. If you get the one on the right, enter Account ID and click "Next"

### RDS Information

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

1\. You are now signed in to the AWS console. Make sure the region selected is "N. Virginia" or "Oregon"

2\. Click on the "RDS" service (You may need to expand "All services"

### RDS Instance

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS7.jpg)

1\. In the left pane click on "Instances"

2\. Click on the RDS instance that corresponds to designated number

### Navigate to Security Groups

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS8.jpg)

1\. Scroll down to the "Details" area and under "Security and network" notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS

2\. Click in the blue hyperlink under "Security groups"

### Security Groups

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

1\. Choose the "rds-launch-wizard-#" RDS Security group corresponding to you (may not match your student number)

2\. After highlighting the appropriate security group click on the "Inbound" tab below

    VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own

### Outbound Traffic

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

1\. Click "Outbound" tab

2\. You can see All traffic (internal to AWS) allowed, this includes your VMware Cloud on AWS SDDC logical networks.

### Elastic Network Interface (ENI)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.

1\. Click on "Services" to go back to the Main Console

2\. Click on "EC2"

### ENI (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

1\. In the EC2 Dashboard click "Network Interfaces" in the left panel

2\. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view type "RDS" in the search area and press Enter to add a filter

3\. Highlight your rds-launch-wizard-# corresponding to your student number based on the second octect of the CIDR block in the last column.

    In this example the CIDR block is 172.6.8.187, this would correspond to student "6"
    
4\. Make note of the "Primary private IPv4 IP" address for the next step

### Photo App

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

1\. On your smart phone (tablet or personal computer), open up a browser and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: 1.2.3.4/Lychee

2\. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:

    Database Host: **x.x.x.x:3306**
    Database Username: **vmware18**
    Database Password: **vmware18**

3\. Click "Connect"

### Login to Photo App

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS14.jpg)

1\. In the upper left part of the browser window click the little arrow to log in.

### Enter Login Information

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)

1\. Type "vmworld" for both user name and password

2\. Click "Sign In"

### Photo Albums

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

Congratulations, you have successfully logged in to the photo app!

OPTIONAL: Feel free to take a picture of the room with your smart phone and upload it to the Public folder.

In summary, the front end (web server) is running in VMware Cloud on AWS as a VM, the back end which is a MySQL database is running in AWS Relational Database Service (RDS) and communicating through the Elastic Network Interface (ENI) that gets established upon the creation of the SDDC.

You have completed the lab. Thanks for stopping by!

