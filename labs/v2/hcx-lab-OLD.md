---
layout: single
title: "Hybrid Cloud Extension (HCX) Lab Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
## Introduction

In this lab exercise you will learn about Hybr Cloud Extension (HCX), Primarily this is a tool, bundled with VMware Cloud on AWS, which will allow you to bulk migrate workloads to VMware Cloud on AWS and significantly reduce the time and complexity of moving workloads into the public cloud sphere.

## What is Hybrid Cloud Extension (HCX)

![HCX1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX1.jpg)

Hybrid Cloud Extension abstracts on-premises and cloud resources and presents them to the apps as one continuous hybrid cloud. On this, Hybrid Cloud Extension provides high-performance, secure and optimized multisite interconnects. The abstraction and interconnects create infrastructure hybridity. Over this hybridity, Hybrid Cloud Extension facilitates secure and seamless app mobility and disaster recovery across on-premises vSphere platforms and VMware Clouds. Hybrid Cloud Extension is a multi-site, multi cloud service, facilitating true hybrid cloud.

### Hybrid Cloud Extension Features

![HCX2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX2.jpg)

Any-to-Any vSphere Cloud App Mobility

• Rapidly move existing workloads from a vSphere platform to the latest SDDC
• Reduce upfront planning time for cost and resource analysis
• Accelerate cloud adoption and avoid retrofitting on-premises environment Business Continuity with Lower TCO

#### Business Continuity with Lower TCO

• IP and MAC address remapping is not required
• No need to retrofit existing VM environment
• Hybrid Cloud Extension provides warm and cold bulk migration, and bidirectional migration
• Hybrid Cloud Extension simplifies your operational model

#### Architected for Security

• Ensure highly secure tethering of private and public clouds
• Protect resources with resilient disaster recovery capabilities
• Hybrid Cloud Extension hybrid DMZ enables portability of enterprise network and security practices to the cloud
• Security policies migrate with applications High-Performance Infrastructure Hybridity

#### High-Performance Infrastructure Hybridity

• In-built WAN optimization is tuned for the needs of hybrid use cases
• Hybrid Cloud Extension provides agile, intelligent routing
• Traffic load balancing overlay is policy-enforced
• Multiple VM migration models (including vMotion, warm, cold) make it easy to migrate to and from the cloud without any changes

## HCX - vMotion Migration

![HCX3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX3.jpg)

1. Open your Chrome browser and click on the *HCX-vMotion* bookmark
2. Click the *X* on the right pane to enlarge the main screen The first tab in the browser demonstrate an on-premises vCenter server.

    ![HCX4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX4.jpg)
3. Click on the second tab, this represents a second data center (This can also represent a VMware Cloud on AWS vCenter)

    ![HCX5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX5.jpg)
4. Click the first tab in the browser to go back to the on-premises vCenter.

    ![HCX6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX6.jpg)
5. Click on the VM named *Mission Critical Workload 1*

    ![HCX7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX7.jpg)
6. Click on the Console screen

    ![HCX8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX8.jpg)

    A console window is now open for the *Mission Critical Workload 1* VM, it will try to ping IP Address 10.159.137.212 which corresponds to a VM in the second site.
7. Click in the tab corresponding to the second site

    ![HCX9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX9.jpg)
8. Click on the VM named *TargetSite-TestVM*

    ![HCX10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX10.jpg)
9. Note the IP address corresponding to this VM is the IP address that the *Mission Critical Workload 1* VM on the source site is trying to ping
10. Click on the *Mission Critical Workload 1* tab

    ![HCX11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX11.jpg)

    • Press enter after the ping command
    • Click *Control-C* to stop ping
    • Type **ping 172.16..4.2** which is this VM's own IP address
11. Click the *X* on this tab to close this tab on your browser

    ![HCX12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX12.jpg)
12. Click on the first tab to go back to the on-premises vCenter

    ![HCX13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX13.jpg)
13. Click on the *Actions* button

    ![HCX14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX14.jpg)
14. Click on *Hybridity Actions*

    ![HCX15](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX15.jpg)
15. Click on *Migrate to the Cloud*

    ![HCX16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX16.jpg)
16. Click on *(Specify Destination Container)*

    ![HCX17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX17.jpg)
17. Select *RedwoodCluster*
18. Click on *Select Destination* button

    ![HCX18](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX18.jpg)
19. Click on *(Select Storage)* button
20. Select *cloudStorage*

    ![HCX19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX19.jpg)
21. Click *(Select Virtual Disk Format)*
22. Select *Same format as source*

    ![HCX20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX20.jpg)
23. Click on *(Select Migration Type)*
24. Select *vMotion*
25. Click *Next*

    ![HCX21](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX21.jpg)
26. Look for the *Validation is Successful* message
27. Click *Finish* button

    ![HCX22](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX22.jpg)
28. Click on the *Home* button
29. Click *HCX* in the left pane
30. Click the *Migration* tab

    ![HCX23](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX23.jpg)
31. Notice the progress of the vMotion migration
32. Click on the *Refresh* tab to update the progress

    ![HCX24](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX24.jpg)
33. Once progress has completed, click on the second tab to open the target site's vCenter

    ![HCX25](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX25.jpg)
34. You now see that *Mission Critical Workload 1* has successfully migrated to the Target Site, click on its name
35. Click on the Console window

    ![HCX26](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX26.jpg)

    • You can see that the ping you left running in a previous step never stopped, successfully vMotioning the VM with zero downtime
    • Press *Control-C* to stop the ping
36. Click on the *X* on the browser tab to close the console tab

### Reverse Migration

![HCX27](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX27.jpg)

1. Click on the first tab in the browser
2. Click the *Migrate Virtual Machines* button

    ![HCX28](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX28.jpg)
3. Click the *Reverse Migration* checkbox

    ![HCX29](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX29.jpg)
4. Click the checkbox for *Mission Critical Workload 1*

    ![HCX30](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX30.jpg)
5. Click on *(Specify Destination Container)*

    ![HCX31](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX31.jpg)
6. Select **Tier 0 Workloads**
7. Click **Select Destination** button

    ![HCX32](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX32.jpg)
8. Click on *(Select Storage)*
9. Select *onpremStorage*
    ![HCX33](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX33.jpg)
10. Click *(Select Virtual Disk Format)*
11. Select *Same format as source*

    ![HCX34](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX34.jpg)
12. Click on *(Select Migration Type)*
13. Select *vMotion*

    ![HCX35](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX35.jpg)
14. Click *Next*

    ![HCX36](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX36.jpg)
15. Once *Validation is Successful* message appears
16. Click *Finish* button

    ![HCX37](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX37.jpg)
17. Check the progress of the migration
18. Click *Refresh* button

    ![HCX38](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX38.jpg)
19. Click the *Home* button
20. Click *Hosts and Clusters* button

    ![HCX39](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX39.jpg)
21. Click on the *Mission Critical Workload 1* VM, you can see that the Reverse migration to the on-premises vCenter was successful

## HCX - Bulk Migration

![Bulk1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk1.jpg)

1. In your Chrome browser click the *HCX-Bulk* bookmark
2. Click on the *X* to enlarge the main screen

    ![Bulk2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk2.jpg)
3. As with the vMotion module, in this example we also have a source site (on-premises) vCenter
4. Click on the second tab in the browser to view the Target site vCenter

    ![Bulk3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk3.jpg)
5. Click on the tab for the on-premises vCenter

    ![Bulk4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk4.jpg)

6. Click on the *Home* button
7. Click *HCX*

    ![Bulk5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk5.jpg)
8. Click on the *Migration* tab

    ![Bulk6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk6.jpg)
9. Click the *Migrate Virtual Machines* button

    ![Bulk7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk7.jpg)
10. Select *Tier 1 Workloads* in the left pane
11. Click the *checkbox* to select all VM's
12. Click *(Specify Destination Container)*

    ![Bulk8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk8.jpg)
13. Select *RedwoodCluster* as the Destination Container
14. Click the *Select Destination* button

    ![Bulk9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk9.jpg)
15. Click *(Select Storage)*
16. Select *cloudStorage*

    ![Bulk10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk10.jpg)
17. Click *(Select Virtual Disk Format)*
18. Select *Same format as source*

    ![Bulk11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk11.jpg)
19. Click *(Select Migration Type)*
20. Select *Bulk Migration*

    ![Bulk12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk12.jpg)
21. Click on *HelpDesk Workload 1* to see the options for this workload
22. Click on *HelpDesk Workload 2* to see the options for this workload
23. Click on the *sidebar* to view all the options
24. Click the *Next* button

    ![Bulk13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk13.jpg)
25. Wait for *Validation is Successful* message
26. Click *Finish* button

    ![Bulk14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk14.jpg)
27. Check Progress of the migrations
28. Click *Refresh* button

    ![Bulk15](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk15.jpg)
29. Once migrations complete, click the *Hosts and Clusters* button

    ![Bulk16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk16.jpg)
30. Click on the second tab to view the cloud vCenter

    ![Bulk17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk17.jpg)
31. You can see that both workloads have successfully migrated to the Target vCenter