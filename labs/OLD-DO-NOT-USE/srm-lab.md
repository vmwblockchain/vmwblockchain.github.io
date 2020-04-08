---
layout: single
title: "Site Recovery Manager (SRM) Lab Manual"
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
# Introduction

In this lab you will pair up with another student group in order to simulate the setup and configuration tasks for VMware Site Recovery Manager

## Activate Site Recovery Add On

Important Instructions for Site Recovery Exercises

PLEASE BE AWARE THAT THESE EXERCISES MUST BE PERFORMED FROM THE ASSIGNED HORIZON DESKTOP YOUR INSTRUCTORS ASSIGNED. IF YOU TRY TO PERFORM SOME OF THE EXERCISES OUTSIDE OF THE HORIZON SESSION YOU WILL EXPERIENCE SOME FAILURES.

### Activate Site Recovery

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM1.jpg)

1\. Click on the *Add ons* tab

2\. Under the Site Recovery Add On, Click the *Activate* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM2.jpg)

3\. In the pop up window Click *Activate* again

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM3.jpg)

Wait until the Site Recovery Add On has been activated. This process should take ~10 minutes to complete.

## What is VMware Site Recovery

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM4.jpg)

VMware Site Recovery brings VMware enterprise-class Software-Defined Data Center (SDDC) Disaster Recovery as a Service to the AWS Cloud. It enables customers to protect and recover applications without the requirement for a dedicated secondary site. It is delivered, sold, supported, maintained and managed by VMware as an on-demand service. IT teams manage their cloud-based resources with familiar VMware tools without the difficulties of learning new skills or utilizing new tools and processes.

VMware Site Recovery is an add-on feature to VMware Cloud on AWS. VMware Cloud on AWS integrates VMware's flagship compute, storage, and network virtualization products: VMware vSphere, VMware vSAN, and VMware NSX along with VMware vCenter Server management. It optimizes them to run on elastic, bare-metal AWS infrastructure. With the same architecture and operational experience on-premises and in the cloud, IT teams can now get instant business value via the AWS and VMware hybrid cloud experience.

The VMware Cloud on AWS solution enables customers to have the flexibility to treat their private cloud and public cloud as equal partners and to easily transfer workloads between them, for example, to move applications from DevTest to production or burst capacity. Users can leverage the global AWS footprint while getting the benefits of elastically scalable SDDC clusters, a single bill from VMware for its tightly integrated software plus AWS infrastructure, and on-demand or subscription services like VMware Site Recovery Service. VMware Site Recovery extends VMware Cloud on AWS to provide a managed disaster recovery, disaster avoidance and non-disruptive testing capabilities to VMware customers without the need for a secondary site, or complex configuration.

VMware Site Recovery works in conjunction with VMware Site Recovery Manager and VMware vSphere Replication to automate the process of recovering, testing, re-protecting, and failing-back virtual machine workloads. VMware Site Recovery utilizes VMware Site Recovery Manager servers to coordinate the operations of the VMware SDDC. This is so that, as virtual machines at the protected site are shut down, copies of these virtual machines at the recovery site startup. By using the data replicated from the protected site these virtual machines assume responsibility for providing the same services.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM5.jpg)

VMware Site Recovery can be used between a customers datacenter and an SDDC deployed on VMware Cloud on AWS or it can be used between two SDDCs deployed to different AWS availability zones or regions. The second option allows VMware Site Recovery to provide a fully VMware managed and maintained Disaster Recovery solution. Migration of protected inventory and services from one site to the other is controlled by a recovery plan that specifies the order in which virtual machines are shut down and started up, the resource pools to which they are allocated, and the networks they can access.

VMware Site Recovery enables the testing of recovery plans, using a temporary copy of the replicated data, and isolated networks in a way that does not disrupt ongoing operations at either site. Multiple recovery plans can be configured to migrate individual applications or entire sites providing finer control over what virtual machines are failed over and failed back. This also enables flexible testing schedules. VMware Site Recovery extends the feature set of the virtual infrastructure platform to provide for rapid business continuity through partial or complete site failures.

## Create a Cross SDDC VPN

We will be setting up an IPSEC VPN connection between your VPC and the VPC of the person you were paired with.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM6.jpg)

1\. Go back to the *VMware Cloud on AWS* tab.

2\. In the main SDDC window, click on *View Details*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM7.jpg)

3\. Click on the *Network* menu

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM8.jpg)

In the Management Gateway section, make a note of the *Public IP* and the *Infrastructure Subnet CIDR*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM9.jpg)

In the Management Gateway settings below

4\. Click the drop down arrow to open the *IPsec VPNs* section

5\. Click on *ADD VPN*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM10.jpg)

Fill in the following information

6\. Name: Student # MGMT GW (where # is your peer's student number)

7\. The *Public IP address* of the persons Gateway you were paired with

8\. The *Infrastructure IP CIDR* of the person you were paired with

9\. Pre-shared key is **VMware1!**

10\. Click on *Save*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM11.jpg)

When both you and the person you were paired with have completed these steps you should see the status of the VPN turn to **Connected**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM12.jpg)

There will be a need to setup a second VPN to our Host infrastructure for this setup to work. This is not normally needed when setting up your on-premises environment but it's needed for the special setup in this workshop.

11\. Make sure the IPSecVPNs drop down is opened, if not click it under *Management Gateway*

12\. Click on *Add VPN*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM13.jpg)

Fill in the following information

13\. Name this VPN **Student# to Host** (where # is your student number)

14\. Enter **54.70.191.234** for the Remote Gateway Public IP

15\. Enter **192.168.30.0/24** under Remote Networks

16\. Pre-shared key is **VMware1!**

17\. Click on *Save*

## Prepare and Pair Site Recovery

### Firewall Rules for Site Recovery

For this module we will utilize the brand new *Firewall Rule Accelerator* option in the VMware
Cloud on AWS portal.

The firewall rule accelerator will create a group of firewall rules for a set of use cases. The
Remote Network of the selected VPN will be used as the source or destination for these rules.
You can edit the rules in the Firewall Rules section after they are created if desired although
there should be no need to edit them.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM14.jpg)

1\. Click on the *Network* tab

2\. Expand the *Firewall Rule Accelerator* area

3\. For Rule Group select *Site Recovery* option

4\. For VPN Select the VPN created against the Student you were paired up with, and you will repeat the same process for the VPN created to the Host infrastructure.

    MAKE SURE TO REPEAT THIS STEP FOR BOTH VPN'S CREATED, THE ONE WITH YOUR PEER STUDENT, AND THE ONE FOR THE HOST.

5\. Click on *Create Firewall Rules*

Watch as the firewall rules get created automatically for you. Once completed, repeat for second VPN, once that one completes you can examine the firewall rules created.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM15.jpg)

### VMware Site Recovery - Site Pairing

*IMPORTANT NOTE*: Only one person can do the Site Pairing exercise. Please decide between you and your partner who performs this step.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM16.jpg)

1\. On your VMware Cloud on AWS Portal click on the *Add Ons* tab

2\. Click *Open Site Recovery*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM17.jpg)

3\. Click on *New Site Pair*

You will be pairing the partner site that was assigned to you by your instructor, note that this is not the information for your SDDC used up until now.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM18.jpg)

This is the information your partner will need from you and you will need from your partner's site.

4\. Click on the *Settings* tab in your SDDC

The username on both sides (yours and your peer) will always be *cloudadmin@vmc.local*

5\. Copy or note the password for the vCenter Server user

6\. Note the URL for the vCenter server and the format it's displayed versus the format it should
be used:

*DISPLAYED*: https://vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com/ui

*USED*: vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM19.jpg)

7\. Make sure your local vCenter is selected

8\. Enter the information from your partner's SDDC:

  PSC host name (make sure to enter the correct format as noted above)

  User name

  Password

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM20.jpg)

9\. Make sure local vCenter server is selected

10\. Select all Services

11\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM21.jpg)

12. Click *Finish* button

### Configure Network Mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM22.jpg)

13\. Click *Network Mappings* in the left pane of the Site Recovery page

14\. Click *New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM23.jpg)

15\. Select *Prepare mappings manually*

16\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM24.jpg)

17\. Expand *SDDC Datacenter* on both sides

18\. Expand *Management Networks* on both sides

19\. Expand *vmc-dvs* on both sides

20\. Select your *Student#-LN* network and your partner's *Student#-LN* (You may need to scroll down to fid these networks)

21\. Click the *Add Mappings* button

22\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM25.jpg)

23\. DO NOT enter or select anything in Reverse Mappings, click *Next*

24\. Leave defaults and click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM27.jpg)

25\. Click *Finish*

### Folder mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM28.jpg)

26\. Select *Folder Mappings* in the left pane

27\. Click *+ New* to create a new folder mapping

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM29.jpg)

28\. Select *Prepare mappings manually*

29\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM30.jpg)

30\. Expand *SDDC Datacenter* on both sides

31\. Select *Workloads* on both sides

32\. Click the *Add Mappings* button

33\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM31.jpg)

34\. DO NOT select any Reverse mappings, click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM32.jpg)

35\. Click *Finish*

### Resource Mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM33.jpg)

36\. Click *Resource Mappings* in the left pane

37\. Click *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM34.jpg)

38\. Expand *SDDC Datacenter* on both sides

39\. Expand *Cluster 1* on both sides

40\. Select *Compute-ResourcePool* on both sides

41\. Click *Add Mappings* button

42\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM35.jpg)

43\. DO NOT select any reverse mappings, click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM36.jpg)

44\. Click *Finish*

### Storage Policy Mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM37.jpg)

45\. Select *Storage Policy Mappings* in the left pane

46\. Click *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM38.jpg)

47\. Select *Prepare mappings manually*

48\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM39.jpg)

49\. Click to select *Datastore Default* on both the left and right pane

50\. Click *ADD MAPPINGS*

51\. Click *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM40.jpg)

52\. Click *Datastore Default* for Reverse mappings

53\. Click *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM41.jpg)

54\. Click *FINISH*

### Placeholder Datastores

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM42.jpg)

55\. Select *Placeholder Datastores* in the left pane

56\. Click *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM43.jpg)

57\. Select *WorkloadDatastore*

58\. Click *Add*

## SRM - Protect a VM

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM44.jpg)

1\. Select a VM to replicate and right-click

2\. Select *All Site Recovery actions*

3\. Click *Configure Replication*

*NOTE*: You may need to log in to the paired site (This is your partner's site), make sure you use *cloudadmin@vmc.local* and get your partner users password. After entering you may need to repeat this step.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM45.jpg)

4\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM46.jpg)

5\. Select the Target Site

6\. If not logged in you may need to login (Remember this is your partner's site not yours)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM47.jpg)

7\. Enter your partners site credentials

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM48.jpg)

8\. Leave all defaults and click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM49.jpg)

9\. Leave the default *Datastore Default* as the VM Storage Policy

10\. Select *WorkloadDatastore*

11\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM50.jpg)

12\. Leave the default 1 hour for Recovery Point Objective, RPO can be as low as 5 minutes, as high as 24 hour

13\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM51.jpg)

14\. Select *Add to new protection group*

15\. Name your Protection Group *PG#* (where # is your student number)

16\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM52.jpg)

17\. Select *Add to new recovery plan*

18\. Name your Recovery Plan **RP#** (where # is your student number)

19\. Click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM53.jpg)

20\. Click *Finish*

## Perform a Recovery Test

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM54.jpg)

1\. In the VMware Cloud on AWS portal, click the *Add Ons* tab

2\. Click on *Open Site Recovery* (You may need to allow Pop-ups in browser)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM55.jpg)

3\. In the Site Recovery window, click *Open*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM56.jpg)

4\. Click on *Recovery Plans*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM57.jpg)

5\. Click on your protection group *PG#* (where # is your student number)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM58.jpg)

6\. Click on *Recovery Plans*

7\. Click on *RP#* which should be your Recovery Plan you created in a previous step

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM59.jpg)

8\. Click the *Test* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM60.jpg)

9\. Leave all defaults and click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM61.jpg)

10\. Click *Finish* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM62.jpg)

11\. Follow the progress in the Recent Tasks area at the bottom of your window

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM63.jpg)

12\. Notice the test has successfully completed

13\. Click the *Cleanup* button to clean up the activity and return the environment to its normal state

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM64.jpg)

14\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM65.jpg)

15\. Click *Finish*, the environment will now be clean. Please note that during testing, your replications protecting your VM's is not interrupted
