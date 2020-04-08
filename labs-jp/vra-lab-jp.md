---
layout: single
title: "vRealize Automation Lab Manual"
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
# Introduction

Very often, customers want to utilise tools which sit above the vCenter API layer and provide additional value to the consumption or onitoring process. One such tool is VMware vRealize Automation which can be utilised to create a governance and "store front" portal so that users can provision workloads where they wish, based on policies defined by the Cloud Administrator.

## What is vRealize Automation (vRA)

VMware vRealize Automation is the IT Automation tool of the modern Software-Define Data Center. vRealize Automation enables IT Automation through the creation and management of personalized infrastructure, application and custom IT services (XaaS). This IT Automation lets you deploy IT services rapidly across a multi-vendor, multi-cloud infrastructure.

- Agility Through IT Automation
- Accelerate the end-to-end delivery and management of infrastructure and applications.
- Choice Through Flexibility
- Provision and manage multi-vendor, multi-cloud infrastructure and applications by leveraging new and existing infrastructure, tools and processes.
- Control Through Governance PoliciesControl Through Governance Policies
- Ensure that users receive the right size resources, or applications, at the appropriate service level for the jobs they need to perform.
- Cost Saving Through EfficiencyCost Saving Through Efficiency Reduce operational cost by replacing time-consuming, manual processes and gain additional cost savings through automated reclamation of inactive resources.

vRealize Automation supports VMware Cloud on AWS in delivering a unified hybrid cloud management experience.

## Login to vRealize Automation

1. On your Student desktop, click on the Chrome browser.
2. On your bookmarks bar in Chrome click on the **VMware vRealize Automation Appliance** link
3. Click on the "Proceed to vraapp.corp.local (unsafe) link
4. Click on the **vRealize Automation Console** link
5. Click **Next**
6. Login with your **vmcws#** user name (where # is your student number)
7. Enter your password - VMware1!

You have successfully logged in to vRealize Automation!

## Add a vSphere Endpoint

An endpoint is anything that vRealize Automation uses to complete it's provisioning processes. This could be a public cloud resource such as Amazon Web Services EC2, Microsoft Azure, VMware Cloud on AWS, an external orchestrator appliance, or a private cloud hosted by vSphere or other hypervisors.

1. Click on the **Infrastructure** tab
2. Click on **Endpoints** on left pane
3. Click on **Endpoints** again on the left pane
4. Click on **New** button
5. Select **Virtual**
6. Select **vSphere (vCenter)**

    For these next few steps, make sure you are logged in to your VMware Cloud on AWS portal.
7. On your VMware Cloud on AWS portal, make sure you click on the **Connection Info** tab, you will be using some of this information to create the vRealize Automation Endpoint
8. Name your vRA Endpoint **ws#** (where # is your student number)
9. Under **Address** enter the information from the VMware Cloud on AWS portal under **vSphere Client (HTML5)** and replace the words 'ui' for 'sdk'
10. Example:
    From VMware Cloud on AWS portal: <https://vcenter.sddc-X-X-X-X.vmc.vmware.com/ui>
    What it should look like in vRealize Automation: <https://vcenter.sddc-X-X-X-X.vmc.vmware.com/sdk>
11. Enter the user name **cloudadmin@vmc.local**
12. Enter the password (Please refer to VMware Cloud on AWS portal for this information)
13. Click on the **Test Connection** button
14. If connection passes test, click on **Ok** button to create your vRealize Automation Endpoint

## Create a Fabric Group

An administrator can organize virtualization compute resources and cloud endpoints into fabric groups by type and intent. Make sure for this step you've selected the Endpoint you created in the previous module.

1. Click on **Fabric Groups** on the left pane
2. Click on "+ New" to create a new Fabric Group
3. Name your Fabric Group **ws#FabricGroup** (where # is your student #)
4. Add the user assigned to your student number: **vmcws#@corp.local** and click on the magnifying glass icon to find and add your user
5. Select the vRealize Automation endpoint you created in the previous step
    NOTE: MAKE SURE YOU SELECT THE CORRECT COMPUTE RESOURCE AS ILLUSTRATED BY THE ARROWS. SHOULD BE THE "WS#" ENDPOINT YOU CREATED IN A PREVIOUS STEP.
6. Click **OK** button

## Reservations

When a user requests a machine, it can be provisioned on any reservation of the appropriate type that has sufficient capacity for the machine. You can apply a reservation policy to a blueprint to restrict the machines provisioned from a that blueprint to a subset of available

### Create Reservation Policy

For this step if your Reservation tab is missing you may need to hit the Refresh button on your browser.

1. Click on the **Infrastructure** button on the left pane
2. Click **Reservations** on the left pane
3. Click **Reservation Policies** on the left pane
4. Click **New** button
5. Name your Reservation Policy "Student#"
6. Click **OK** button

### Create a Reservation

A vRealize Automation reservation is a means to allocate resources in a fabric group (CPU, RAM, Storage, etc.) to a specific business group.

1. Click on the **nfrastructure** button on the left pane
2. Click on **Reservations** on the left pane
3. Click **Reservations** on the left pane
4. Click **+ New** to create a new Reservation
5. Select **vSphere (vCenter)**
6. Give your reservation a name: **Student#** (where # is your student number assigned to you)
7. Leave/select vsphere.local as the Tenant
8. Select **vmc-ws#** (where # is your student number)
9. For Reservation policy type or select **Student#** (where # is your student number assigned to you)
10. Priority should be set to **1**
11. Click on the **Resources** tab
12. On the **Resources** tab for **Compute Resource** select **Cluster-1 (WS#)** from the drop down box
13. Type **1024** in the **This Reservation** field
14. Select the checkbox for **WorkloadDatastore**
15. Type **10000** under the **This Reservation Reserved** field
16. Type **0** under the Priority field and hit the **OK** button
17. Select **Compute-ResourcePool** in the **Resource Pool** field
18. Click on the **Network** tab
19. Select the **sddc-cgw-network-1** Network Adapter by clicking the checkbox to the left of it and selecting **sddc-cgw-network-1** from the dropdown box on the right side
20. Click on the **OK** button

## Create a Blueprint

A blueprint is a complete specification for a service. A blueprint determines the components of a service, which are the input parameters, submission and read-only forms, sequence of actions and provisioning. You can create service blueprints to provision custom resources that you previously created.

1. Click on the **Design** tab
2. On the left pane click **Blueprints**
3. Click the **+ New** button
4. Name your Blueprint **Student#** (where # is the Student number assigned to you)
5. Use the same name for ID
6. Enter **1** as the Deployment Limit
7. Enter **1** as the Minimum Lease days
8. Enter **1** as the Maximum Lease days
9. Enter **0** for Archive Days
10. Click the **OK** button, the Design Canvas below appears
11. Ensure **Machine Types** is selected
12. Click and drag onto the Canvas the **vSphere (vCenter) Machine**
13. Leave all defaults on the **General** tab and click on the **Build Information** tab
14. Make sure **Server** is selected as the Blueprint Type
15. Change Action to **Clone**
16. Click on the elipsis button and select **StudentVM#** that you created in your previous step and click **OK**
17. Click **Save**
18. Click **Finish**
19. Select your Blueprint by highlighting it
20. Click on the **Publish** button to publish your blueprint
21. Make sure you are in the **Administration** tab, if not, click on it and select **Catalog Management**
22. Select **Catalog Items** from the left pane and click your **Student#** to configure
23. Select **Desktops** from the drop down box next to **Service**
24. Click the **OK** button

### Entitle your Blueprint

1. Click the **Administration** tab
2. Select **Catalog Management** from the left pane
3. Click on **Entitlements** on the left pane
4. Click the **+ New** button in order to create a new Entitlement
5. Name the Entitlement **ws#** (where # is your Student #)
6. Change the Status to **Active**
7. Select **vmc-ws#** for the Business Group (where # is the number assigned to you)
8. Click **Next**
9. Click the **+** sign next to **Entitled Services**
10. Make sure to select the checkbox next to **Desktops**
11. Click **OK**
12. Click on the "+" sign next to **Entitle Actions**
13. Select the checkbox at the top to select all Actions
14. Click **OK** button
15. Click **Finish** button

### Request a catalog item

1. Select the **Catalog** tab
2. Click on your newly created **Student#** blueprint and click on the **Request** button
3. Highlight the **vSphere_VCenter_Machine** under your Blueprint
4. Check all the options are correct and click the **Submit** button
5. Click on the **Requests** tab to check on the status of your Blueprint submission
6. Check the status to ensure the request completes successfully