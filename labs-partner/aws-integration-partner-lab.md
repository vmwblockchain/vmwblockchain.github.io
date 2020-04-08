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

In this lab we will work through some common basic integrations which you can utilise in your VMware Cloud on AWS platform.

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs-partner/working-with-sddc-partner-lab/) concerning Content Library creation and Network creation and firewall rule creation.

## AWS Relational Database Service (RDS) Integration

### Deploy Photo VM

As a first step in setting up our integration between the VMware vSphere platform in VMware Cloud on AWS and native AWS services, we are going deploy a VM which we will use for this demo. This VM will be referred to as "Photo VM". Please follow the instructions below.

1. If not already opened, open your VMware Cloud on AWS vCenter and click on the **Menu** drop down
2. Select **Content Libraries**
3. Click on your previously created Content Library named **Student#** (where # is your student number)
4. Make sure you click on the **Template** tab
5. Right-click on the **photo app** Template
6. Select **New VM from This Template**
7. Name the virtual machine **PhotoApp#** (where # is your student #)
8. Expand the location and select **Workloads**
9. Click **Next**
10. Expand the destination to select **Compute-ResourcePool** as the compute resource
11. Click **Next**
12. In the "Review details" step click **Next**
13. In the **Select storage** step, highlight the "WorkloadDatastore"
14. Click **Next**
15. Select the network you created in a previous step for your VM
16. Click **Next**
17. Click **Finish**
18. Monitor the deployment of your VM until it's completed
19. Check for completion of the deployment of your VM
20. Click **Menu**
21. Select **VMs and Templates**
22. Check to make sure your VM is powered on. If not, right-click on your VM
23. Select **Power** -> **Power On**
24. Make sure your VM is assigned an IP addresses. (may need to a few minutes for this information to be populated). Make a note of this IP address for a future step.

### Firewall Rules for RDS Integration

In this step we will ensure that we have the correct firewall rules in place in order for our Photo App VM in VMware Cloud on AWS to talk across to the RDS service in our AWS VPC.

1. Go back to your VMware Cloud on AWS portal and click on the **Network & Security** tab in order to request a **Public IP address**
2. Under the **System** section click **Public IPs**
3. Click on **Request New IP** on the notes section enter "Student#" (Where  # is the student number assigned to you) then hit save. Your public IP rquest should look similar to the screenshot below. 
    ![Rquest New IP](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/request-new-ip.jpg)
    
    Note: You can request public IP addresses to assign to workload VMs to allow access to these VMs from the internet. VMware Cloud on AWS will provision the IP address from AWS.

4. Take note of your newly acquired Public IP Addres
5. Next you will create a **NAT rule** from the newly acquired public IP address you noted in your last step to the internal IP address of the VM you created. Click on  **NAT** under the "Network" section to expand the NAT Rules. 
6. Click **ADD RULE**
7. Name your NAT rule "Student#" (Where # is the student number assigned to you)
8. Select your Public IP address
9. Under **Service** select **Any (All Traffic)**
10. Type your VM's internal IP address
11. Save your rule. 
    Your NAT rule should look similar to the screenshot below. 
    ![NAT Rule](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/nat.jpg)

Now we will create a firewalll rule to allow access from the Internet into our photo VM.

15. Expand **Gateway Firewall** under the Security section and select **Compute Gateway**
16. Click **ADD NEW RULE**
17. Name your Firewall Rule "Student# Photo Inbound" 
18. Select **Any** for **Source**
19. For **Destination** click **CREATE A NEW GROUP** 
    1. Name the group "PhotoAPP#" (Where # is the number assigned to your student number)
    2. For **Member Type** select **Virtual Machine**
    3. Select your PhotoVM. 
    4. Click **SAVE**
    5. Click **SAVE**
20. For **Services** Select "Any"
21. Click **Publish** button on the top right corner of the screen
22. You should get a **Firewall rule successfully created** notification.
    Your firewal rule should look similar to the screensho below
    ![Inbound Photo VM](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/photo-inbound.jpg)

### AWS Relational Database Service (RDS Configuration)

On your browser, open a new tab and go to: <https://vmcworkshop-partner.signin.aws.amazon.com/console> if your org starts from PL-1 through PL-10. If your org starts from 
SL-1 through SL-10 then log into <https://wwcp-console.signin.aws.amazon.com/console> ask your instructor for more information if you are having difficulties identifying your org number. 

1. For Account ID or alias ensure **vmcworkshop-partner** is specified for orgs PL-1 through Pl-10 for orgs SL-1 through SL-10 ensure **wwcp-console** is specified as the alias.
2. IAM user name - Student# (where # is the number assigned to you)
3. Password - VMCworkshop2019!
4. Click **Sign In**
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon** in the top right hand corner of the AWS Console
6. Click on the **RDS** service under the "Database" section
7. In the left pane click on **Instances**
8. Click on the RDS instance that corresponds to your Student number
9. Scroll down to the **Details** area and under **Security and network** notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS
    ![RDS Public Access](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/Screenshot+at+Jul+20+12-38-34.png)
10. Go back to the main Services page in the AWS console by clicking the **Services** link in the top left corner of the console
11. Scroll down to **Networking & Content Delivery** and click **VPC**
12. Click on **Security Groups** in the left pane under the "Security" section
13. Choose the **rds-launch-wizard-#** RDS Security group corresponding to your student number (where # is the number assigned to you)
14. After highlighting the appropriate security group click on the **Inbound Rules** tab below. VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own
15. Notice that the CIDR block range of your Student#-LN Logical Network you created in VMware Cloud on AWS is authorized for MySQL on port 3306. This was done for you ahead of time
16. AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.
17. Click on **Services** in the top left of the console to go back to the Main Console
18. Click on **EC2** under the Compute section
19. In the EC2 Dashboard click **Network Interfaces** in the left pane under the "Network & Security" section
20. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view, type "RDS" in the filter area and press **Enter** to add a filter
21. Highlight your **rds-launch-wizard-#** corresponding to your student number (where # is the number assigned to you)
22. Make note of the **Primary private IPv4** IP address for the next step
23. Open an additional browser tab and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: x.x.x.x/Lychee
24. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:
    **Database Host**: x.x.x.x:3306
    **Database Username**: student# (where # is the number assigned to you)
    **Database Password**:VMware1!
25. Click **Connect**

You have now successfully created a Hybrid Application utilises components across both your VMware on AWS SDDC environment and your AWS services.

This functionality provides customers now with choices around how applications are migrated to the cloud. You can now split your application across platforms and consume services in vSphere and AWS as it makes sense. This level of choice can really help those who are looking to migrate to the cloud, accelerate that process by not getting "bogged down" in refactoring parts of an application which are difficult to move to hyperscale cloud.

## AWS Elastic File System (EFS) Integration

### EFS VM Creation

In our next section on integrating AWS services with VMware Cloud on AWS. We will utilise the AWS Elastic File System (EFS) which we can use to mount NFS shares to our VMware Cloud on AWS hosted virtual machines.

1. Navigate to your Content Library, click **Menu** on your VMware Cloud on AWS vCenter Server
2. Select **Content Libraries**
3. Click on your **Student#** Content Library
4. Make sure the **Templates** tab is selected
5. Right-click on the **efs** template
6. Select **New VM from This Template**
7. Name your VM **EFSVM#** (where # is your student number)
8. Select **Workloads** for the location of your VM
9. Click **Next**
10. Select **Compute-ResourcePool** as the destination for your VM
11. Click **Next**
12. Click **Next**
13. Select **WorkloadDatastore** for storage
14. Click **Next**
15. Select your Destination Network
16. Click **Next**
17. Click **Finish**
18. Make sure to Power on your VM and ensure it has an assigned private IP address

### AWS Elastic File System (EFS)

On your browser, open a new tab and go to: <https://vmcworkshop-partner.signin.aws.amazon.com/console> if your org starts from PL-1 through PL-10. If your org starts from 
SL-1 through SL-10 then log into <https://wwcp-console.signin.aws.amazon.com/console> ask your instructor for more information if you are having difficulties identifying your org number. 

1. For Account ID or alias ensure **vmcworkshop-partner** is specified for orgs PL-1 through Pl-10 for orgs SL-1 through SL-10 ensure **wwcp-console** is specified as the alias.
2. IAM user name - Student# (where # is the number assigned to you)
3. Password - **VMCworkshop2019!**
4. Click **Sign In**
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon**
6. Click on the **EFS** service under the storage section
7. Select your Student# NFS (where # is the number assigned to you. Students 11 through 20 are assigned shares 1 through 10. For example Student 11 has share 1 Student 12 has share 2 and student 20 has share 10)
8. Note the IP address
9. Back on your vCenter Server tab, click on **Launch Web Console**  for your EFS VM (Might need to allow pop ups in browser). Log in using the following credentials:
 a. **User**: root
 b. **Password**: VMware1!

Enter the following commands at the prompt:

``` bash
cd /
mkdir efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs Where MOUNT_TARGET_IP is the IP you noted for your EFS
cd efs
touch hello.world
ls
```

You have now integrated AWS EFS with a VM running in VMware Cloud on AWS.