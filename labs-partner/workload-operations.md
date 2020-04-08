---
layout: single
title: "VMware Cloud on AWS Workload Operations"
date: 2018-07-18
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
## Introduction
Go through the basic day 2 operations of managing the virtual infrastructure inside your SDDC. In this lab you will deploy several windows machines and will be performing operations such as customizing the OS and monitoring cluster performance. 

The virtual machines deployed in this lab are a requirement for your Site Recovery lab. 

## Task 1 Create Customization Specifications

1.  Log into your vCenter Web Client > click the **Menu** icon and slect **Policies and Profiles**
    
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/1.jpg)

2.  Under **Policies and Profiles**, click VM Customizations Specifications and click "Create a new specification icon. 

![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/2.jpg)

3. On the Name and target OS page, enter Student#Win10-CustomSpec in the Customization Spec Name text box. 
4. Under the Guest OS, verify that windows is selected as the Target Guest OS. 
5. Verify that Generate a new security identity (SID) is selected

![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/3.jpg)

6. Click Next

7. On the Registration Information page, enter VMware Student Number in the Owner name text box and enter VMware in the Owner organization text box. 

![Screenshot}](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/4.jpg)

8. On the Computer Name Page, select **Use the virtual machine name** and click **Next**
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/5.jpg)

9. On the **License Page**, leave the product key text box blank, leave other settings at their defaults, and click **Next**
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/6.jpg)

10. On the **Administrator Password** page, enter the password **VMware1!** and verify it. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/7.jpg)

11. Click **Next**

12. On the **Time Zone** page, select Pacific Time, from the Time Zone drop-down menu and click next. 

13. On the **Commands to run once** page, click Next. 

14. On the **Network page**, verify that Use standard network settings for the guest Operating system, including enabling DHCP on all network interfaces is selected and click **Next**. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/8.jpg)

15. On the **Workgroup or Domain page**, verify that "Workgroup" is clicked and that the text box shows "WORKGROUP". 

16. Click "Next"

17. On the **Ready to Complete** page, review the infromation and click "Finish". 

18. In the **Customization Specification Manager** pane, verify that Win10-CustomSpec is listed. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/9.jpg)

## Task 2 Deploy a Virtual Machine from a Template

You use templates to rapidly deploy and provision new virtual machines and easily customize the guest operating systems. 

1. Navigate to the content library you registered in lab "Working with your SDDC" and navigate to the template section. 

2. Right click on the "windows-10" template and select New VM from this Template. The Deploy from template wizard opens. 

3. On the Select name and folder page, enter "Win10-##" Where ## is the student number assigned to you. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/11.jpg)

4. On the "Select location for the virtual machine" page, expand VMC on AWS Datacenter and select the "worklod VMs" folder. 

5. Click "Next"

6. On the "Select a Compute Resource" page, expand the VMC on AWS Datacenter > and "Compute ResourcePool"

7. Click "Next"  

8. On the "Select Storage" page, select "Compute-Datastore" from the Storage Compatability: Compatible list. The Compatability pane displays the Compatability checks succeeded message. 

9. Click "Next" 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/12.jpg)

10. On the "Select Clone Options" page, select the "Customize the operating system" and the "Power on virtual machine after creation" check boxes and click "Next" 

11. On the "Customize guest OS" page, select Win10-CustomSpec## and click "Next". 

12. On the "Ready to complete" page, review the information and click "Finish". 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/13.jpg)

13. Repeat steps 1 thorugh 13 to create another virtual machine. The second machine should be named Win10-recovery##. The ## is the student number assigned to you. We will use this VM to failover in our Site Recovery lab. 

14. Monitor the progress of the 2 deplooyed virtual machines  and wait for completion. 

## Task 3 Modifying Virtual Machine and Virtual Hardware. 

In the following exercises we will modifying the hard drive space and the vRAM of the deployed VMs. 

### Modifying Virtual Machine Hard Drive 

1. Right-click on your Win10-## virtual machine in the Navigator and select "Edit Settings" 

2. On the "Virtual Hardware" tab, record the size (GB) of the Hard Disk1. 
    ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/14.jpg)

3. In the "Hard disk 1" text box, increase the disk size by 2 GB and click "ok". 
    ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/15.jpg)

4. On the "Win10-##" Summary tab, select launch "Web Console. 

5. If you are not logged into windows go ahead and log in as Administrator/VMware1!

6. In this step we will be extending our disk to the increased disk space. 
    a. Right-Click on "This PC" and Select "Manage" 
    ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/16.jpg)

    b. Click on "Disk Management" 
    ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/17.jpg)

    c. In the lower right pane, verify that the 2GB unaloccated disk space is discovered
    ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/18.jpg)

    If the 2.00 GB of unallocated space has not been discovered, click the Action menu and select "Rescan Disk"

    d. Click "Next"

    e. on the "Select Disks" page, verify that Disk 0 is selected in the "Selected" pane and click Next. 

    f. On the "Completing the Extend Volume" Wizard page, review the information and click finish. 

7. In the "Disk Management" window, verify that the local c: drive (Disk 0) is extended. 

8. close the "Disk Management window" and close the Virtual Machine Console. 

### Adjusting Memory Allocation on a Virtual Machine

vSphere allows you to add or modify the virtual machine memory once the virtual machine has been deployed. This could be done to enhance virtual machine performance or to righ size VM based on it's workloads. 

1. Right-click the Win10-## virtual machine and select Power > Shutdown Guest OS from the Navigator pane. 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/19.jpg)

2. Click "Yes" to verify the shutdown

3. On the Win10-Clone Summary tab, expand the view of the VM Hardware pane and record the amount (GB) of active memory

4. Right-click the Win10-## virtual machine in the Navigator pane and select "Edit Settings" 
![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/20.jpg)

5. Enter 4500 in the Memory text box and verify that MB is selected from the drop-down menu. 

6. Click "Ok"

7. Verify that the memory is increased on the virtual machine's Summary tab. 



