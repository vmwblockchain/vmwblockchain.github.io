---
layout: single
title: "VMware Cloud on AWS VPN Lab Manual"
date: 2018-07-18
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
## Introduction
Configure a VPN to provide a secure connection to your SDDC over the public Internet or AWS Direct Connect. Route-based and policy-based VPNs are supported. Either type of VPN can connect to the SDDC over the Internet. A route-based VPN can also connect to the SDDC over AWS Direct Connect.

In this lab exercise we will be showing how you can establish network connectivity to another SDDC hosted on VMC on AWS. We will go through the steps to configure a policy based L3VPN.

**Note** This lab requires the interaction of only one group member per SDDC. Plase decide between you and your partner who will performing this step

This lab is a prequisite for the Site Recovery Lab 

## SDDC Pairings

Given that we only have 10 SDDC's we will be pairing SDDC's 1 through 5 with SDDC's 6 through 10. 

SDDC-1 Paired with SDDC-6

SDDC-2 Paired with SDDC-7

SDDC-3 Paired with SDDC-8

SDDC-4 Paired with SDDC-9

SDDC-5 Paired with SDDC-10

## L3VPN Coniguration

Two types of L3VPN configurations are configurable on VMC on AWS, **Route Based** and **Policy Based** **VPN**. 

### Route Based VPN 
A route-based VPN creates an IPsec tunnel interface and routes traffic through it as dictated by the SDDC routing table. A route-based VPN provides resilient, secure access to multiple subnets. When you use a route-based VPN, new routes are added automatically when new networks are created.

Route based VPNs in your VMware Cloud on AWS SDDC use an IPsec protocol to secure traffic and the Border Gateway Protocol (BGP) to discover and propagate routes as new networks are created. To create a route-based VPN, you configure BGP information for the local (SDDC) and remote (on-premises) endpoints, then specify tunnel security parameters for the SDDC end of the tunnel.

### Policy Based VPN
A policy-based VPN creates an IPsec tunnel and a policy that specifies how traffic uses it. When you use a policy-based VPN, you must update the routing tables on both ends of the network when new routes are added.

Policy-based VPNs in your VMware Cloud on AWS SDDC use an IPsec protocol to secure traffic. To create a policy-based VPN, you configure the local (SDDC) endpoint, then configure a matching remote (on-premises) endpoint. Because each policy-based VPN must create a new IPsec security association for each network, an administrator must update routing information on premises and in the SDDC whenever a new policy-based VPN is created. A policy-based VPN can be an appropriate choice when you have only a few networks on either end of the VPN, or if your on-premises network hardware does not support BGP (which is required for route-based VPNs).

### Configuring a Policy Based BPN 
In this exercise we will be configuring a policy based VPN and exposing our mgmt networks needed for the Site Recovery Lab. 

1. If you are not already logged in log in to the VMC Console at <https://vmc.vmware.com>.
2. Select **Networking & Security > VPN > Policy Based**.
3. Click **ADD VPN** and give the new VPN a name simillar to **student 1 to student 6"**
4. Select **Public IP Adrress** as your **Local IP Address** for the VPN. 
5. Enter the **Remote Public IP** address of your assigned target SDDC (Get this information from your peer assigned to your paired SDDC)
6. **Skip this Step** If your on-premises gateway is behidn a NAT device, enter the gateway address as the **Remote Private IP**. This IP address must match the local identity (IKE ID) sent by the on-premises VPN gateway. If this field is empty, the **Remote Public IP** field is used to match the local identity of the on-premises VPN gateway. 
 **Note:** We will not be performing this step since the MGW and CGW (Compute and Management gateways are not behind a NAT)
7. Specify the **Remote Networks** that this VPN can connect to. In this step enter the remote management network of your paired SDDC. **For Example** if you are paired with SDDC#6 the management network should be 10.6.0.0/16. 
8. Specify the **Local Networks** that this VPN can connect to. For this step enter your local management network. **For Example** if you are assigned to SDDC 1 your management network should be 10.11.0.0/16 also labeled as **Infrastructure Subnet**. 
9. Configure Advanced Tunnel Parameters:
    Tunnel Encryption: **AES 256**
    Tunnel Digest Algorithm: **SHA 2**
    Perfect Forward Secrecy: **Enabled**
    Preshared Key: **VMware1!**
    IKE Encryption: **AES 256**
    IKE Digest Algorithm: **Sha 2**
    IKE Type: **IkE V2**
    Diffie Hellman: **Group 14**
10. Click **Save**

**Results**
The VPN creation process might take a few minutes. If the VPN was configured correctly on both sides the **status** should be "up". 
![Status](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/vpn+screenshots/PolicyVPN+up.jpg)

