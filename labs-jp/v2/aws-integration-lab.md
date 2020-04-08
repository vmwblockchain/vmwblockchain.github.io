---
layout: single
title: "AWS Integration Lab Manual"
categories: labs
date: 2019-05-09
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
<!--
This documentation is translated into Japanese from following commit.
commit 6d44840734c66796e7f5ad6208873826dede03de
-->

<!--
# Introduction

One of the most compelling reasons to adopt VMware Cloud on AWS is to integrate your existing systems which sit in your VMware cloud environment, with application platforms which reside in your AWS Virtual Private Cloud (VPC) environment. The intergration which VMware and AWS have created allows for these services to communicate, for free, across a private network address space for services such as EC2 instances, which connect into subnets within a native AWS VPC, or with platform services which have the ability to connect to a VPC Endpoint, such as S3 Storage.

## Understanding Integrations with AWS Services

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-1.jpg)

As the above diagram illustrates, the VMware stack not only sits next to the AWS services, but is tightly integrated with these services. This introduces a new way of thinking about how to design and leverage AWS services with your VMware SDDC. Some integrations our customers are using include:

* VMware front-end and RDS backend
* VMware back-end and EC2 front-end
* AWS Application Load Balancer (ELBv2) with VMware front-end (pointing to private IPs)
* Lambda, Simple Queueing Service (SQS), Simple Notification Service (SNS), S3, Route53, and Cognito
* AWS Lex, and Alexa with the VMware Cloud APIs

These are only a few of the integrations we've seen. There are many different services that can be integrated into your environment.
In this exercise we'll be exploring integrations with both AWS Simple Storage Service (S3) and AWS Relational Database Service (RDS).

**Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs/v2/working-with-sddc-lab/) concerning Content Library creation, Network creation, and Firewall Rule creation.**
-->

# はじめに

VMware Cloud on AWS を採用する理由の一つとして、AWS の Virtual Private Cloud (VPC) 環境にあるアプリケーションプラットフォームと、VMware Cloud on AWS の環境にある既存のシステムを統合することが挙げられます。VMware と AWS のこの統合は、これらのアプリケーションサービスとの通信を可能にします（通信コストは無償）。ネイティブ AWS VPC のサブネットにある EC2 インスタンスといったサービスのプライベートネットワークアドレススペースを介して可能となります。また、S3 ストレージなどの VPC Endpoint との接続によるプラットフォームサービスとも可能です。

## AWS サービスとの統合の理解

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-1.jpg)

上の図にあるように、VMware のスタックは AWS のサービスのとなりにあるだけでなく、これらのサービスに密接に統合されています。VMware SDDC と一緒に AWS のサービスをデザイン、活用するための新しい方法をもたらします。既に顧客が利用している統合には以下があります:

* VMware SDDC をフロントエンドとし、AWS RDS をバックエンドとしたシステム
* VMware SDDC をバックエンドとし、AWS EC2 をフロントエンドとしたシステム
* AWS のアプリケーションロードバランサー (ELBv2) と VMware SDDC のフロンドエンドを組み合わせたシステム （プライベート IP をポイント）
* Lambda、Simple Queueing Service (SQS)、Simple Notification Service (SNS)、S3、Route53、、Cognito と VMware SDDC を組み合わせたシステム
* AWS Lex、Alexa と VMware Cloud の API を組み合わせたシステム

これらは、顧客が実現した統合の一部でしかありません。このほかにも、皆様の環境に統合できるたくさんのサービスがあります。この演習では、AWS Simple Storage Service (S3) と AWS Relational Database Service (RDS) と VMware Cloud SDDC を組み合わせたシステムを扱います

**注意: このラボを行う要件として、コンテンツライブラリ作成、ネットワーク作成、ファイアウォールルール作成を含む、[SDDC の操作](https://vmc-field-team.github.io/labs/v2/working-with-sddc-lab/)を完了している必要あります。**

<!--
### How are these integrations possible?

In addition to sitting within the AWS Infrastructure, there is an Elastic Network Interface (ENI) connecting VMware Cloud on AWS and the customer's Virtual Private Cloud (VPC), providing a high-bandwidth, low latency connection between the VPC and the SDDC. This is where the traffic flows between the two technologies (VMware and AWS). There are no EGRESS charges across the ENI within the same Availability Zone and there are firewalls on both ends of this connection for security purposes.

### How is traffic secured across the ENI?

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-2.jpg)

From the VMware side (see image below), the ENI comes into the SDDC at the Compute Gateway (NSX Edge). This means, on this end of the technology we allow and disallow traffic from the ENI with NSX Firewall rules. By default, no ENI traffic can enter the SDDC. Think of this as a security gate blocking traffic to and from AWS Services on the ENI until the rules are modified.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-3.jpg)

On the AWS Services side (see image below), Security Groups are utilized. For those who are not familiar with Security Groups, they act as a virtual firewall for different services (VPCs, Databases, EC2 Instances, etc). This should be configured to deny traffic to and from the VMware SDDC unless otherwise configured.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-4.jpg)

In this exercise, everything has been configured on the AWS side for you. You will however walk through how to open AWS traffic to come in and out of your VMware Cloud on AWS SDDC.
-->

### 統合の実現方法

AWS のインフラストラクチャ上に SDDC があるだけでなく、顧客の Virtual Private Cloud (VPC) と VMware Cloud on AWS を接続する Elastic Network Interface (ENI) によって実現されます。ENI は、SDDC と VPC 間で広帯域幅、低遅延の接続を提供します。ENI により 2 つのテクノロジー (VMware と AWS) 間でトラフィックが流れることになります。同一アベイラビリティーゾーンでの ENI 越しの通信には EGRESS チャージが掛かりません。また、セキュリティを目的に、この接続の両端ではファイアウォールがあります。

### ENI 越しの通信の安全性

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-2.jpg)

VMware 側から見ると、ENI は SDDC にコンピュートゲートウェイ（NSX Edge）から入ってきます。これは、こちら側の終端では ENI からのトラフィックの許可、不許可を NSX ファイアウォールで制御できることを意味します。デフォルトでは、ENI からのトラフィックは SDDC に流れてきません。ルールを変更するまでは、これは ENI を通じた AWS サービスから、あるいは、AWS サービスへのトラフィックをブロックするセキュリティのゲートとして考えて下さい。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-3.jpg)

AWS サービス側から見ると、セキュリティグループが利用されます。セキュリティグループについて詳しくない方に向けた説明として、これは様々なサービス（VPC、データベース、EC2インスタンスなど）仮想ファイアウォールとして振る舞います。これは、設定されていない場合を除き、VMware SDDC から、SDDC へのトラフィックを拒否する設定であるべきです。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-4.jpg)

この演習では AWS 側は設定済みとなりますが、VMware Cloud on AWS の SDDC へ、SDDC からの AWS のトラフィックをどのようにオープンするかを確認していきます

<!--
### Compute Gateway Firewall Rules for Native AWS Services

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-5.jpg)

1. In the VMware Cloud on AWS portal click the **Networking & Security** tab
2. Click **Groups** in the left pane
3. Click **ADD GROUP**

#### Name Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-6.jpg)

1. Type **PhotoAppVM** for the Name
2. Leave **Virtual Machine** select for Member Type
3. Click **Set VMs** under Members

#### Select VMs - Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-7.jpg)

1. Click to select **Webserver01**
2. Click **SAVE**

#### Save Group - Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-8.jpg)

1. Click **SAVE**
-->

### ネイティブ AWS サービスのための コンピュートゲートウェイファイアウォール ルール

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-5.jpg)

1. VMware Cloud on AWS のポータルの **ネットワークとセキュリティ** タブをクリックします
2. 左ペインの **グループ** をクリックします
3. **グループの追加** をクリックします

#### ワークロード グループの命名

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-6.jpg)

1. グループの名前に **PhotoAppVM** を入力します
2. メンバーのタイプは **Virtual Machine** のままとします
3. メンバーの下の **仮想マシンの設定** をクリックします

#### 仮想マシンの選択 - ワークロード グループ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-7.jpg)

1. **Webserver01** を選択するためにクリックします
2. **SAVE** をクリックします

#### グループの保存 - ワークロード グループ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-8.jpg)

1. **SAVE** をクリックします

<!--
### Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-9.jpg)

1. Click **Networking & Security** tab in your VMware Cloud on AWS Portal
2. Click **Gateway Firewall** in the left pane
3. Click and select **Compute Gateway**
4. Click **ADD NEW RULE**

#### Add New Rule - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-10.jpg)

1. Name your new rule **AWS Inbound**
2. Click on **Set Source**

#### Select Source - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-11.jpg)

1. Click to select **Connected VPC Prefixes**
2. Click **SAVE**

#### Set Destination - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-12.jpg)

1. Click on **Set Destination**

#### Select Destination - AWS Inbound (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-13.jpg)

1. Click to select **PhotoAppVM**
2. Click **SAVE**

#### Set Service - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-14.jpg)

1. Click on **Set Service**

#### Set Service - AWS Inbound (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-15.jpg)

1. Click to select **Any**
2. Click **SAVE**

#### Publish - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-16.jpg)

1. Click on **PUBLISH**

**Note:** Make sure to leave **All Uplinks** in the **Applied To** section.
-->

### ファイアウォール ルール

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-9.jpg)

1. VMware Cloud on AWS のポータルで **ネットワークとセキュリティ** タブをクリックします
2. 左ペインの **ゲートウェイファイアウォール** をクリックします
3. **コンピュートゲートウェイ** をクリックします
4. **新しいルールの追加** をクリックします

#### 新しいルールの追加 - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-10.jpg)

1. 新しいルールの名前を **AWS Inbound** と入力します
2. **送信元の設定** をクリックします

#### 送信元の選択 - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-11.jpg)

1. **Connected VPC Prefixes** をクリックします
2. **SAVE** をクリックします

#### 宛先の設定 - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-12.jpg)

1. **宛先の設定** をクリックします

#### 宛先の選択 - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-13.jpg)

1. **PhotoAppVM** をクリックします
2. **SAVE** をクリックします

#### サービスの設定 - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-14.jpg)

1. **サービスの設定** をクリックします

#### サービスの設定 - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-15.jpg)

1. **Any** をクリックします
2. **SAVE** をクリックします

#### 発行 - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-16.jpg)

1. **発行** をクリックします

**注意:** **適用先** セクションが **All Uplinks** のままであることを確認します

<!--
#### Add New Rule - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-17.jpg)

1. Click **ADD NEW RULE**
2. Name your new rule **AWS Outbound**
3. Click on **Set Source**

#### Select Source - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-18.jpg)

1. Click to Select **PhotoAppVM**
2. Click **SAVE**

#### Set Destination - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-19.jpg)

1. Click on **Set Destination**

#### Select Destination - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-20.jpg)

1. Click to select **Connected VPC Prefixes**
2. Click **SAVE**

#### Set Service - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-21.jpg)

1. Click on **Set Service**

#### Set Service - AWS Outbound (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-22.jpg)

1. Under **Select Services** type **3306**
2. Select **MySQL** checkbox
3. Click **SAVE**

#### Publish - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-23.jpg)

1. Click **PUBLISH**

**Note:** Make sure to leave **All Uplinks** in the **Applied To** section.
-->

#### 新しいルールの追加 - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-17.jpg)

1. **新しいルールの追加** をクリックします
2. ルールの名前を **AWS Outbound** と入力します
3. **送信元の設定** をクリックします

#### 送信元の選択 - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-18.jpg)

1. **PhotoAppVM** をクリックします
2. **SAVE** をクリックします

#### 宛先の設定 - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-19.jpg)

1. **宛先の設定** をクリックします

#### 宛先の選択 - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-20.jpg)

1. **Connected VPC Prefixes** をクリックします
2. **SAVE** をクリックします

#### サービスの設定 - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-21.jpg)

1. **サービスの設定** をクリックします

#### サービスの設定 - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-22.jpg)

1. **サービスの選択** の下に **3306** と入力します
2. **MySQL** のチェックボックスをクリックします
3. **SAVE** をクリックします

#### 発行 - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-23.jpg)

1. **発行** をクリックします

**注意:** **適用先** セクションが **All Uplinks** のままであることを確認します

<!--
### Add New Rule - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-24.jpg)

1. Click on **ADD NEW RULE**

#### Add New Rule - Public In (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-25.jpg)

1. Type **Public In** for Name
2. Click on **Set Source**

#### Select Source - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-26.jpg)

1. Click to select **Any**
2. Click **SAVE**

#### Set Destination - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-27.jpg)

1. Click on **Set Destination**

#### Select Destination - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-28.jpg)

1. Click to select **PhotoAppVM**
2. Click **SAVE**

#### Set Service - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-29.jpg)

1. Click **Set Service**

#### Set Service - Public In (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-30.jpg)

1. Type **HTTP 80** under **Select Services**
2. Click to Select **HTTP**
3. Click **SAVE**

#### Publish - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-31.jpg)

1. Click **PUBLISH**

**Note:** Make sure to leave **All Uplinks** in the **Applied To** section.
-->

### 新しいルールの追加 - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-24.jpg)

1. **新しいルールの追加**

#### 新しいルールの追加A - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-25.jpg)

1. ルールの名前に **Public In** と入力します
2. **送信元の設定** をクリックします

#### 送信元の選択 - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-26.jpg)

1. **Any** をクリックします
2. **SAVE** をクリックします

#### 宛先の設定 - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-27.jpg)

1. **宛先の設定**

#### 宛先の選択 - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-28.jpg)

1. **PhotoAppVM** をクリックします
2. **SAVE** をクリックします

#### サービスの設定 - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-29.jpg)

1. **サービスの設定** をクリックします

#### サービスの設定 - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-30.jpg)

1. **サービスの選択** の下に **HTTP 80** と入力します
2. **HTTP** をクリックします
3. **SAVE** をクリックします

#### 発行 - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-31.jpg)

1. **発行** をクリックします

**注意:** **適用先** セクションが **All Uplinks** のままであることを確認します

<!--
## AWS Relational Database Service (RDS) Integration

Amazon RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost- efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching and backups. It frees you to focus on your applications so you can give them the fast performance, high availability, security and compatibility they need.

In this exercise, you will be able to integrate a VMware Cloud on AWS virtual machine to work in conjunction with a relational database running in Amazon Web Services (AWS) that has been previously setup on your behalf.

### Make Note of Webserver01 IP Address

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

You will be using the VM created in the previous module in order to complete this exercise.

1. In your vCenter interface for VMware Cloud on AWS, find your **Webserver01** VM you deployed, and ensure it has been assigned an IP address as shown in the graphic.

### Assign Public IP

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

1. Go back your VMware Cloud on AWS portal and click on the **Networking & Security** tab in order to request a Public IP address
2. Click **Public IPs** in the left pane
3. Click on **REQUEST NEW IP**
4. In the notes area type **PhotoAppIP**
5. Click **SAVE**

### Note New Public IP

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

Take note of your newly created Public IP.

### Create a NAT Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

1. Click **NAT** in the left pane
2. Click **ADD NAT RULE**
3. Type **PhotoApp NAT** for Name
4. Ensure the Public IP you requested in the previous step appears under Public IP
5. Leave **All Traffic** (no change)
6. Type the IP address of your **Webserver01** VM you noted at the beginning of this exercise
7. Click **SAVE**
-->

## AWS Relational Database Service (RDS) との統合

Amazon RDS は、クラウドでのデータベースのセットアップ、運用、スケールを簡易にします。ハードウェアのプロビジョニング、データベースセットアップ、パッチ適用、バックアップ取得といった時間の掛かる管理タスクを自動化しながらも、コスト効果が高く、さらにリサイズ可能になっています。これにより、よりアプリケーションにフォーカスすることができ、アプリケーションに必要な高いパフォーマンス、高可用性、セキュリティ、互換性を提供できます。

この演習では、VMware Cloud on AWS の仮想マシンを Amazon Web Services (AWS) で事前にセットアップされたリレーショナルデータベースと共に動くように構成します

### Webserver01 の IP アドレスをメモ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

この演習のために、前のモジュールで作成した仮想マシンを利用します

1. VMware Cloud on AWS の vCenter のインターフェースで、デプロイ済みの **Webserver01** という仮想マシンを見つけます。そして図にあるように IP アドレスが割り当てられていることを確認します

### Public IP の割当

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

1. VMware Cloud on AWS のポータルに戻り、Public IP アドレスをリクエストするために **ネットワークとセキュリティ** タブをクリックします
2. 左ペインの **パブリック IP アドレス** をクリックします
3. **新しい IP アドレスの要求** をクリックします
4. **PhotoAppIP** と入力します
5. **SAVE** をクリックします

### 新しいパブリック IP のメモ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

新しく用意されたパブリック IP をメモします

### NAT ルールの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

1. 左ペインの **NAT** をクリックします
2. **ルールの追加** をクリックします
3. 名前に **PhotoApp NAT** を入力します
4. パブリック IP アドレス セクションの下に、前のステップでリクエストしたパブリック IP があることを確認します
5. **All Traffic** のままとします (変更しません)
6. メモしてある仮想マシン **Webserver01** の IP アドレスを入力します
7. **SAVE** をクリックします

<!--
### AWS Relational Database Service (RDS) Integration

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS24.jpg)

On your browser, open a new tab and go to: https://vmcworkshop.signin.aws.amazon.com/console

1. Account ID or alias - Please refer to the information on the card provided to you for Account ID information
2. IAM user name - **Student#** (where # is the number assigned to you)
3. Password - **VMCworkshop1211**
4. Click **Sign In**

Please note you might get either of the 2 sign on screens above. If you get the one on the right, enter Account ID and click **Next**

### RDS Information

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

1. You are now signed in to the AWS console. Make sure the region selected is **Oregon**
2. Click on the **RDS** service (You may need to expand **All services**)

### RDS Instance

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS07.jpg)

1. In the left pane click on **Databases**
2. Click on the RDS instance that corresponds to designated number
-->

### AWS Relational Database Service (RDS) との統合

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS24.jpg)

ブラウザの新しいタブで https://vmcworkshop.signin.aws.amazon.com/console に移動します

1. アカウント ID またはエイリアスは、渡されたカードの情報を参照します
2. IAM ユーザー名 - **Student#** (# は受講者番号)
3. Password - **VMCworkshop1211**
4. **サインイン** をクリックします

<!-- Please note you might get either of the 2 sign on screens above. If you get the one on the right, enter Account ID and click **Next** -->
<!-- ここは訳す必要なし。上の画面がアップデートされているので 2 sign on screens above の意味が通じなくなっている-->

### RDS の情報

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

1. これで AWS のコンソールにサインインしました。選択されているリージョンが **オレゴン** であることを確認します
2. **RDS** サービスをクリックします (**サービス** をクリックする必要があるかも知れません)

### RDS インスタンス

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS07.jpg)

1. 左ペインの **データベース** をクリックします
2. 受講者番号に対応する RDS インスタンスをクリックします

<!--
### Navigate to Security Groups

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS08.jpg)

1. Scroll down to the **Details** area and under **Connectivity & security** notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS
2. Click in the blue hyperlink under **Security groups**

### Security Groups

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

1. Choose the **Student##-RDS-Inbound** RDS Security group corresponding to you (may not match your student number)
2. After highlighting the appropriate security group click on the **Inbound** tab below

**Note: VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own**

### Outbound Traffic

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

1. Click **Outbound** tab
2. You can see All traffic (internal to AWS) allowed, this includes your VMware Cloud on AWS SDDC logical networks.
-->

### セキュリティグループへの移動

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS08.jpg)

1. 右ペインをスクロールダウンさせ、**接続とセキュリティ** で RDS インスタンスにパブリックアクセシビリティがないことを確認します。これはこのインスタンスが AWS からのみアクセスできることを意味します
2. **せキュリティグループ** の下の青いハイパーリンクをクリックします

### セキュリティグループ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

1. **Student##-RDS-Inbound** という RDS のセキュリティグループを選択します (おそらく受講者番号とは一致していません)
2. 適切なセキュリティグループをハイライトさせたのち、下部の **インバウンド** タブをクリックします

**Note: VMware Cloud on AWS は、デフォルトの VPC セキュリティグループにルーティングを確立します。RDS はこれを用いることもでき、また独自のセキュリティグループを持つこともできます。**

### アウトバウンドのトラフィック

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

1. **アウトバウンド** タブをクリックします
2. 全てのトラフィック（SDDC 内部から AWS）が許可されていることが確認できます。これは VMware Cloud on AWS の論理ネットワークが含まれます

<!--
### Elastic Network Interface (ENI)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.

1. Click on **Services** to go back to the Main Console
2. Click on **EC2**

### ENI (Continued)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

1. In the EC2 Dashboard click **Network Interfaces** in the left panel
2. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view type **RDS** in the search area and press Enter to add a filter
3. Highlight your **Student##-RDS-Inbound** security group corresponding to your student number based on the second octect of the CIDR block in the last column.

    In this example the CIDR block is 172.6.8.187, this would correspond to student **6**
    
4. Make note of the **Primary private IPv4 IP** address for the next step
-->

### Elastic Network Interface (ENI)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

AWS Relational Database Service (RDS) は、VMware Cloud on AWS が作成する ENI とは別にアクセスするための自身の Elastic Network Interface を作成します

1. メインコンソールの **サービス** をクリックします
2. **EC2** をクリックします

### ENI

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

1. EC2 ダッシュボードの左ペインにある **ネットワークインターフェース** をクリックします
2. 同じ AWS アカウントに属する全ての受講者環境が表示されるため、数百の ENI があるかも知れません。リストを短くするために **RDS** とサーチウィンドウエリアに入力してフィルターを追加します
3. プライマリプライベート IPv4 アドレスの 2 オクテット目をベースとした受講者番号に対応するセキュリティグループ **Student##-RDS-Inbound** をハイライトさせます

    この図の例のプライマリプライベート IPv4 アドレスの IP アドレスは 172.6.8.187、これに対応する受講者番号は **6** となります
    
4. 次のステップのために **プライマリプライベート IPv4 アドレス** をメモします

<!--
### Photo App

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

1. On your smart phone (tablet or personal computer), open up a browser and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: 1.2.3.4/Lychee
2. Enter the database connection information below (__case sensitive__), using the IP address you noted in the previous step from the RDS ENI:

    Database Host: x.x.x.x:3306
    Database Username: student# (where # is the number assigned to you)
    Database Password: VMware1!

3. Click **Connect**

### Enter Login Information	

 ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)	

 1. Type **student#** (where # is the number assigned to you) for user name and **VMware1!** for password.	
 2. Click **Sign In**

### Photo Albums

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

Congratulations, you have successfully logged in to the photo app!

OPTIONAL: Feel free to take a picture of the room with your smart phone and upload it to the Public folder.

In summary, the front end (web server) is running in VMware Cloud on AWS as a VM, the back end which is a MySQL database is running in AWS Relational Database Service (RDS) and communicating through the Elastic Network Interface (ENI) that gets established upon the creation of the SDDC.

You have completed the lab. Thanks for stopping by!
-->

### Photo App

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

1. スマートフォンでブラウザを起動し、VMware Cloud on AWS のポータルでリクエストしたパブリック IP アドレスを入力し、続いて /Lychee (大文字小文字識別) を入力します。例: 1.2.3.4/Lychee
2. 以下の DB 接続情報を入力します (__大文字小文字を識別__)。IP アドレスは前のステップでメモした RDS の ENI のものを利用します

    Database Host: x.x.x.x:3306

    Database Username: student# (# は受講者番号)
    
    Database Password: VMware1!

3. **Connect** をクリックします

### ログイン情報の入力

 ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)	

 1. ユーザー名に **student#** (# は受講者番号) を入力し、パスワードに **VMware1!** を入力します
 2. **Sign In** をクリックします

### Photo Albums

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

おめでとうございます、無事 Photo App にログインできました !

オプショナル: スマートフォンで自由に部屋を撮影し、その写真を Public Folder にアップロードしてみてください

ここまでのステップで学んだことをを簡単に要約すると、フロントエンド（Web Server）は VMware Cloud on AWS の VM で稼働し、バックエンド（MySQL データベース）は AWS Relational Database Service (RDS) で稼働し、それらは SDDC 作成時に一緒に作成された Elastic Network Interface (ENI) により通信が行われます。

これでこのラボは完了しました。演習に取り組んで頂きありがとうございました！

