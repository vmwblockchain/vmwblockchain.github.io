---
layout: single
title: "Site Recovery Lab"
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

This lab envrionment will configure a cloud to cloudd DR scenario. We will not be pairing Site Recovery with an on premise environment, however such configuratiton is also possible. 

**Group Pairings** 
Since we only have 10 SDDDCs in our environment we will be pairing in groups of 2 SDDC's per team.

SDDC1 - SDDC6
SDDC2 - SDDC7
SDDC3 - SDDC8
SDDC4 - SDDC9
SDDC5 - SDDC10

## What is VMware Site Recovery

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM4.jpg)

VMware Site Recovery brings VMware enterprise-class Software-Defined Data Center (SDDC) Disaster Recovery as a Service to the AWS Cloud. It enables customers to protect and recover applications without the requirement for a dedicated secondary site. It is delivered, sold, supported, maintained and managed by VMware as an on-demand service. IT teams manage their cloud-based resources with familiar VMware tools without the difficulties of learning new skills or utilizing new tools and processes.

VMware Site Recovery is an add-on feature to VMware Cloud on AWS. VMware Cloud on AWS integrates VMware's flagship compute, storage, and network virtualization products: VMware vSphere, VMware vSAN, and VMware NSX along with VMware vCenter Server management. It optimizes them to run on elastic, bare-metal AWS infrastructure. With the same architecture and operational experience on-premises and in the cloud, IT teams can now get instant business value via the AWS and VMware hybrid cloud experience.

The VMware Cloud on AWS solution enables customers to have the flexibility to treat their private cloud and public cloud as equal partners and to easily transfer workloads between them, for example, to move applications from DevTest to production or burst capacity. Users can leverage the global AWS footprint while getting the benefits of elastically scalable SDDC clusters, a single bill from VMware for its tightly integrated software plus AWS infrastructure, and on-demand or subscription services like VMware Site Recovery Service. VMware Site Recovery extends VMware Cloud on AWS to provide a managed disaster recovery, disaster avoidance and non-disruptive testing capabilities to VMware customers without the need for a secondary site, or complex configuration.

VMware Site Recovery works in conjunction with VMware Site Recovery Manager and VMware vSphere Replication to automate the process of recovering, testing, re-protecting, and failing-back virtual machine workloads. VMware Site Recovery utilizes VMware Site Recovery Manager servers to coordinate the operations of the VMware SDDC. This is so that, as virtual machines at the protected site are shut down, copies of these virtual machines at the recovery site startup. By using the data replicated from the protected site these virtual machines assume responsibility for providing the same services.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM5.jpg)

VMware Site Recovery can be used between a customers datacenter and an SDDC deployed on VMware Cloud on AWS or it can be used between two SDDCs deployed to different AWS availability zones or regions. The second option allows VMware Site Recovery to provide a fully VMware managed and maintained Disaster Recovery solution. Migration of protected inventory and services from one site to the other is controlled by a recovery plan that specifies the order in which virtual machines are shut down and started up, the resource pools to which they are allocated, and the networks they can access.

VMware Site Recovery enables the testing of recovery plans, using a temporary copy of the replicated data, and isolated networks in a way that does not disrupt ongoing operations at either site. Multiple recovery plans can be configured to migrate individual applications or entire sites providing finer control over what virtual machines are failed over and failed back. This also enables flexible testing schedules. VMware Site Recovery extends the feature set of the virtual infrastructure platform to provide for rapid business continuity through partial or complete site failures.

## Activate Site Recovery Add On

Important Instructions for Site Recovery Exercises

**Note:** Activating the Site Recovery add on is an operation done per SDDC, therefore only one team member per SDDC should perform this step. Please decide which team member will be activating site recovery. 

1. If you have not already done so log into your SDDC at <https://vmc.vmware.com/console/sddcs> and open your SDDC
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/1.jpg)

2. Navigate to **Add Ons** and click **Activate** "Site Recovery"
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/2.jpg)

3. Verify "Default extension ID" is selected and click **Acitvate**

4. The activation of the service should take aboutt 10 to 15 min. 


## Configuring Network Connectivity for Site Recovery

Site Recovery Components are installed on the management resource pool inside your SDDC. These are accessed behind the management gateway and do not have public IP addresses assigned to them but rather consume private infrastructure IP space  definedd during the deployment of the SDDC.

Pairing Site Recovery to an on premise or cloud environment requires layer 3 connectivity either by IPSecVPN or Ddirect Connect. We have established this network connectivity in our previous "VPN-lab" If you have not completed this lab you will not be able to proceed. 

### Management Gateway Firewall Configuration

To enable VMware Site Recovery on your SDDC environment, you must create firewall rules between your on-premises or cloud data center and the Management Gateway.

**Note:** Only one student should open the firewall rules. Please decide between you and your partner who will be configuring the firewall rules for Site Recovery)

**Note:** You must enable Site Recovery on your SDDC before proceeding. 

**Rules Required:**
  1. Allow inbound service HTTPS (TCP 433) to vCenter (This rule should have been created on lab "Working with your SDDC)
  2. Allow inbound service SRM Server Management (TCP 9086) to Site Recovery Manager
  3. Allow inbound service VR Server Management (TCP 8043) to vSphere Replication
  4. Allow outbound service Any (All Traffic) from vCenter, Site Recovery Manager, and vSphere Replication

**Procedure:**
1. Select Networking & Security > Edge Firewall > Management Gateway.
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/4.jpg)

2. Select Add New Rule > Name the rule Inbound SRM > for source enter any > for destination go to system defined and select site recovery manager >  for services select VMware Site Recovery SRM > set Action to Allow > Publish the Firewall rule. Your rule should look like this
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/5.jpg)

3. Select Add New Rule > Name the rule inbound VR  > for source enter any > for destination go to system defined and select 
vSphere Replication > for services select VMware Site Recovery vSphere Replication > set Action to Allow  > Publish the Firewall rule. Your rule should look like this. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/6.jpg)

4. Select Add New Rule > Name the rule Outbound SRM > for source select Site Recovery Manager > for destination select any for services select any > for action select allow > Publish your firewall rule. Your rule should look like this. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/7.jpg)

5. Select Add New Rule > Name the rule Outbound VR > for source select vSphere Replication > for destination select any > for services select any > for action select allow > Publish your firewall rule. Your rule should look like this. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/8.jpg)


**Note**  Opening up your firewall to any source and destination  is not a recommended practice  for production environments. It is recommended you allow traffic only to your on premises environments or to another cloud SDDC for Site Recovery  Configuration. 

### VMware Site Recovery

*IMPORTANT NOTE*: Only one person can do the Site Pairing exercise. Please decide between you and your partner who performs this step.

**Note**  As mentioned before Site Recovery Components are only accessible behind the Management Gateway Firewall, therefore we will be u sing our Windows10 VM inside our SDDC for the following steps. 

#### Accessing your Windows 10 VM

1. Log into your vCenter's SDDC and open a web console to your win10-## VM you created in the operations lab. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/9.jpg)

2. Log into windows as "desktop-admin/VMware1!"

3. Click the "Start Button" and type "Change Ethernet Settings" and click on the icon. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/10.jpg)

4. Click on  "Change Adapter Options" 

5. Right click on "Ethernet 0"

6. Select TCP/IPV4 and click on "Properties"

7. Click "Obtain IP Address Automatically"

8. Ensure DNS server is set to 8.8.8.8 and click OK and close

9. Your Widnows10 VM  should  now  have access to the internet. Open  a web browser in your Windows10 VM and log into <https://cloud.vmware.com>

10. Log in with  my VMware Credentials

11. Click on "Console" on the top right corner
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/site+recovery+screenshots/11.jpg)

12. 

### Site Pairing

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
