---
layout: single
title: "VMware Cloud on AWS API Lab Manual"
date: 2018-07-18
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introduction

In this lab exercise we will be showing how you can intereact with the VMware Cloud on AWS platform through programmatic means. We will go through how we can use PowerShell as a means to interact with the Cloud Solution Platform as well as the vCenter instance. We will then delve into how we can interact with the VMware Cloud on AWS REST API and perform actions in both the interegrated "Developer Center" view in the console, and also through popular third party and open source REST clients. For the purposes of our lab exercise we will be making use of "Postman" as our REST Client.

## Using PowerShell

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs1.jpg)

1\. Click on **Start**, and scroll down until you see the Windows PowerShell menu

2\. Right click on the **PowerShell** CLI shortcut icon and select **Run as Administrator**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs3.jpg)

Install the VMware PowerCLI module

```powershell
Install-Module VMware.PowerCLI
```

**NOTE**: You will be asked to install the NuGet provider, take the default or press **Y** and press
enter, you will then be asked to trusted an untrusted repository, **DO NOT** take the default but type **Y** and press Enter.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs4.jpg)

We now need to set the execution policy to Remote Signed.

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Force
```

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs5.jpg)

You now will need to set the PowerCLI Configuration to Ignore Invalid Certificates.

**IMPORTANT STEP:**

```powershell
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false
```

**NOTE**: Be sure the "i" in "Ignore" is capitalized if you are not using copy/paste

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs6.jpg)

We now need to install the VMware CLI commands

```powershell
Install-Module -name VMware.VMC -scope AllUsers -Force
```

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs7.jpg)

Let's take a quick look at the VMware CLI commands.

```powershell
Get-VMCCommand
```

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs8.jpg)

We now need to get your Refresh Token from the VMC console. Switch back to or open the web
browser and log into **vmc.vmware.com**.

If you are not already logged in

4\. open a new tab

5\. Click on the VMware Cloud on AWS shortcut

6\. Fill in your email address

7\. Click on **Next**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs9.jpg)

8\. Click on the drop down next to your **Name/Org ID**

9\. Click on *My Account*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs10.jpg)

We will now create a new Refresh Token for the ID linked to this Org

10\. Click on *API Tokens* tab

11\. Click *NEW TOKEN*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs11.jpg)

12\. Click on *Create a new token*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs12.jpg)

13\. Click on *Copy to Clipboard*

Now let's attach to the VMC server

```powershell
connect-vmc -refreshtoken xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

*NOTE*: Within the PowerShell window you can just right-click to paste the code, quotes are optional

*NOTE*: Paste the refresh token you copied earlier in the exercise

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs13.jpg)

Now we can see what Orgs we have access to using the following command

```powershell
Get-VMCorg
```

14\. Note the Org Display_Name and ID

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs14.jpg)

Now that we know the Org Display_Name we can find out information about the SDDC's inside
our org.

**NOTE**: replace # with your workstation number

```powershell
Get-VMCSDDC -Org VMC-WS#
```

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs15.png)

Another cool thing you can do is see the Default Credentials for your SDDC

```powershell
Get-VMCSDDCDefaultCredential -org VMC-WS#
```

**NOTE**: replace # with your workstation number

## REST APIs through Developer Center

In this module we will be using the VMware Cloud on AWS REST API to get some basic information about your VMware Cloud on AWS Organization and SDDC deployment. To do this we will be using the new Developer Center feature in VMware Cloud on AWS. This was built specifically to focus on using APIs and scripts to create SDDCs, add and remove hosts, plus connect to and use the full vCenter API set. To get started, let go back to your VMC environment.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter1.jpg)

Launch the Chrome browser on your Student View Desktop

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter2.jpg)

If you are not already logged in, log into your VMware Cloud on AWS organisation.

1\. From within the VMware Cloud on AWS tab, click on the Developer Center menu

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter3.jpg)

In the Developer Center there are a lot of great resources for you to explore. For example, let's check out a code sample that was uploaded by one of our API developers. If you scroll through this screen you will see there are code samples for Postman (a REST API Development Tool)

You will also find samples for Python, PowerCLI, and many others. Anyone can contribute code samples to the community, if that interests you go to <http://code.vmware.com> or click on the link **VMware{code} Sample Exchange**.

2\. Click on *Code Samples* in the menu

3\. Click on *Download* in the "PowerCLI - VMC Example Script" box

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter4.jpg)

After the script downloads

4\. Click on the dropdown arrow

5\. Click on *Show in Folder*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter5.jpg)

6\. Right click on the downloaded script

7\. Click on edit

This will open the PowerShell ISE environment. Now you can see the PowerShell commands
you used in the previous module as well as other commands you can use with your SDDC.

Close the PowerShell ISE windows

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter6.jpg)

Let's now run some simple REST API commands built into Developer Center, go back to your
browser

8\. Click on the API Explorer menu

9\. Make sure you select your SDDC

10\. Click on the drop down arrow next to Organization

11\. Click on the drop down arrow next to the first "GET" API

12\. Click on *Execute*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter7.jpg)

What did we not do?? We did not put in any authentication to pull this data. The reason is we
are using the session authentication to execute these commands. To run these commands in
other application, like PowerShell or Postman, you will need to get your resource and session
tokens before you can run these commands.

Let's look through the response.

13\. Here you see the Organization's alphanumeric name. Which you can also find in *\#3*

14\. The organization *ID*. *NOTE*: Copy the ID number, without the quotes, for possible use in the next step.

15\. The organization *Display_Name*

16\. The organization Version

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter8.jpg)

In this step, we will GET some information about our organization

17\. Click on the drop down arrow by SDDCs

18\. Click on *GET*

19\. The Org ID should already be filled in for you, another great feature the developers built in
based on customer feedback. *NOTE*: If this Org ID did not automatically fill in, paste it in.

20\. Click on *Execute*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter9.jpg)

Now let's look at the response body

21\. The creation date of the SDDC

22\. The SDDC ID

23\. the SDDC state

## Postman

In this module, we will be exploring how to use Postman to execute REST API requests and build automation through collections. Postman is an API Explorer tool. As an example, you can create variables for use within the APIs, test the response, and use webhooks to integrate with collaboration platforms.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman1.jpg)

Postman is very easy to install, so let's get started.

1\. Open a new browser tab and go to <https://www.getpostman.com>

2\. Click on *Download the App*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman2.jpg)

3\. Select Postman for **Windows (64-bit)**. Click **Download**. Double-click on the downloaded file, the install will execute without interaction.

*NOTE*: For cleanup you can close all postman tabs in Chrome

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman3.jpg)

4\. Click on the text: *Skip Signing in and Take me straight to the app*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman4.jpg)


5\. Uncheck *Show this window on launch*

6\. Close this window

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman5.jpg)

Go back to your browser window, if you do not have a tab opened for VMware Cloud on AWS, follow the below instructions

7\. Open a new tab
  Navigate to <https://vmc.vmware.com>
  Log in with you email address which you used to register for the VMware Cloud on AWS Experience Day
  Username : **corp\vmcws#** (your student number)
  Password : **VMware1!**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman6.jpg)

Our internal API development team has done a great job pre-creating SDKs for many of the popular languages in use today. For this module, we will be using the SDK for REST to show you how you can easily import and reuse some pre-built collections to create your own.

8\. Click on *Developer Center*

9\. Click on *View Source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman7.jpg)

10\. Click on the download menu

11\. Click on *Open*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman8.jpg)

12\. Click on *Extract*

13\. Click on *Extract all*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman9.jpg)

We will keep the default file path.

14\. Uncheck the box

15\. Click on *Extract*

Close the file explorer window

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman10.jpg)

Now that we have Postman installed and our REST samples on our local system, lets import the VMC collection and use some the requests to build our own collection.

1\. Click on *Import*

2\. Click on *Choose Files*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman11.jpg)

To import the VMC collection json file we downloaded earlier.

18\. Browse to the directory we extracted the zip file to earlier. That directory should be *C:\downloads\vsphere-automation-sdk-rest-master\vsphere-automation-sdk-rest-master\samples\postman*

19\. Click *VMware Cloud on AWS APIs.postman_collection.json*

20\. Click *Open*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman12.jpg)

We now need to get our refresh token for our Org in VMC. Go back to your VMware Cloud on AWS tab in your browser

21\. Click on the drop down next to your *Name/Org ID*

22\. Click on *My Account*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman13.jpg)

23\. Click on *API Tokens* tab

24\. Click *NEW TOKEN*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman14.jpg)

Now we create a refresh token for your ID tied to this Org

25\. Click on *Continue*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman15.jpg)

26\. Click on *Copy to Clipboard*
*NOTE*: If you have not generated a token yet, click on Generate and then copy to clipboard.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman16.jpg)

Return to the Postman app. We now need to setup a Postman environment for use with VMC.
An environment is where we will be creating and storing our variables. These variables can be local or global, depending on your use within Postman. In this module, we will only be using
local variables.

27\. Click on *New*

28\. Click on *Environment*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman17.jpg)

29\. Name the environment **VMC**

30\. In the Key column type in **refresh_token**

31\. In the Value column use CTRL-V to paste your actual refresh token you copied in a previous
step.

32\. Click on *Add*

33\. Close the window

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman18.jpg)

Now set this as our default environment.
*NOTE*: If you don't set the default environment to *VMC*, then the variables that get created will not be accessible.

34\. Click on the drop down arrow

35\. Select *VMC*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman19.jpg)

Now we will start to build our own collection by using some request that came in the SDK we
imported earlier.

36\. Click on *Collections*

37\. Click on - *Authentication and Login*

38\. See how this request is our refresh token variable we defined in an earlier step.
*NOTE*: If the environment is not set to VMC, this will request will fail because the refresh_token variable is not defined.

39\. Click on *Send*

40\. You will now see the access token that was generated with the refresh token. This is the
body or payload of the response to our request.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman20.jpg)

41\. Click on the Eye icon

You will see that we have stored your access token into a variable so we can use it for future
calls. How did we do that? We ran a "test" on the response body. You will see how in the next
step.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman21.jpg)

42\. Click on *Tests*

The access_token variable was set by running some java script code against the response. We
are also using the Postman setEnvironmentVariable function to create it.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman22.jpg)

Lets save this request to our own collection so we can use it later.

43\. Click on the drop down arrow

44\. Click on *Save As*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman23.jpg)

45\. Change the Request name to *Authorize*

46\. Change the Request description to *Get Access Token*

47\. Click on *+Create Collection*

48\. Type **Workshop** and click the *check box*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman24.jpg)

49\. Select the *Workshop* folder

50\. Click on *Save to Workshop*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman25.jpg)

A new window will pop open indicating that you created a new collection. We will not do
anything here at this time.

51\. Close this window

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman26.jpg)

Let's request some details from our Org so we can send them to Slack.

52\. Click on *Orgs* and *List Orgs*

53\. Click on *Headers*

54\. Click *Send*

55\. You see here how we are using the **access_token** variable for the **csp-auth-token**. This will authorize our request. *NOTE*: This access token is only good for 30 minutes. If you run this request and get a response of **400 unauthorized**, go back and run the authorize request.

56\. Look through the response body for your Org's **display_name**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman27.jpg)

Let's save this request to our own collection so we can use it later.

57\. Click on the drop down arrow

58\. Click on *Save As*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman28.jpg)

59\. Change the Request name to **Org list**

60\. Change the Request description to **Get a list of your Orgs**

61\. Be sure *Workshop* is selected under **Select a collection or folder to save to:**

62\. Click on *Save to Workshop*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman29.jpg)

We need to replace the Test code that came with the SDK so we can create variable we want to
use when send our message to Slack.

63\. Click on Tests
    Copy and paste the below code into the **Tests** section. *NOTE*: You may have to press CTRL-V to paste into the text box.

64\. Click *Send*

var jsonData = JSON.parse(responseBody);

 if (responseCode.code === 200) {
   for (i = 0; i < jsonData.length; i++) {
      pm.environment.set("name", jsonData[i].display_name);
      pm.environment.set("ID", jsonData[i].id);
      pm.environment.set("version", jsonData[i].version);
      pm.environment.set("state", jsonData[i].project_state);
   }
 }

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman30.jpg)

We can verify if the variables have been created and assigned values.

65\. Click on the eye icon

66\. Scroll down to see if the new variables were created.
    Once verified click on the "eye" icon again to close the window

    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman31.jpg)

Lets save the changes we made to this request.

67\. Click on *Save*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman32.jpg)

Now that we have details of our Org lets send them to slack inn a message.

To post to slack a link needs to be generated for the slack channel that we want to post to. This
has already been done for you and is listed below. One of the instructors will have this slack
channel displayed on the screens. So you can see the results.

Slack channel URL: https://hooks.slack.com/services/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

Now we need to setup the request:

68\. Click on the **+** sign for a new request

69\. Change the request type to *POST*

70\. Cut and paste the above slack channel URL to the *address* box

71\. Select *Body*

72\. Change the format type to *raw*

73\. Type the below code, or cut and paste it into the Body section. *NOTE*: You may have to
press CTRL-V to past into the text box.

{
  "text" : "Your Org ID is: {{ID}}\nYour Org version is: {{version}}\nAnd your Org state is: {{state}}",
  "username" : "{{name}}"
}

74\. Click *Send*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman33.jpg)

Lets save this request to our own collection so we can use it later.

75\. Click on the drop down arrow

76\. Click on *Save As*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman34.jpg)

77\. Change the Request name to **Post to Slack**

78\. Change the Request description to **Post some Org details to slack**
    Be sure Workshop is selected under *Select a collection or folder to save to:*

79\. Click on *Save to Workshop*

Check and see if your request posted the Name, ID, Version, and Status of your Org.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman35.jpg)

The last thing to show you with Postman is the way that you can run a collection to automate a
series of tasks. What we have been doing in this module is building a collection. As you see in
the screen shot there are 3 tasks in the Workshop collection.

80\. Click on the Arrow in the Workshop window

81\. Click on *Run*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman36.jpg)

82\. Click on *Run Workshop*

83\. Be sure the **Environment** is set to VMC

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman37.jpg)

If all your work was saved and ran individually, they should run here as well.

84\. Check out the status of each request.

If you have all "200 OK" then you will see another post in slack for your workshop Org.

Please add comments below if you would like to give feedback on this lab.
