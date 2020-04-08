---
layout: single
title: "Site Recovery Manager (SRM) ラボ マニュアル"
date: 2018-09-29
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
<!-- based on commit:e5f130c1242f3d2a87bd47e00ee8909c014423d2-->

<!--
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
-->

# はじめに

このラボでは、VMware Site Recovery Manager のセットアップおよび構成タスクをシミュレートするために、他の受講者グループとペアを組むことになります。

## Site Recovery アドオンのアクティベート

Site Recovery の実習のための大事な確認項目

これらの演習が、インストラクターに割り当てられた Horizon デスクトップから行われていることを確認してください。もし Horizon デスクトップでないところからこの演習を行おうとした場合、失敗する可能性があります。

### Site Recovery のアクティベート

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM1.jpg)

1\. *Add ons* タブをクリックします

2\. Site Recovery Add On において、*Activate* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM2.jpg)

3\. ポップアップ ウィンドウにて *Activate* をもう一度クリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM3.jpg)

Site Recovery アドオンがアクティベートされるまで待機します。このプロセスは 10 分程度で完了します

<!--
## What is VMware Site Recovery

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM4.jpg)

VMware Site Recovery brings VMware enterprise-class Software-Defined Data Center (SDDC) Disaster Recovery as a Service to the AWS Cloud. It enables customers to protect and recover applications without the requirement for a dedicated secondary site. It is delivered, sold, supported, maintained and managed by VMware as an on-demand service. IT teams manage their cloud-based resources with familiar VMware tools without the difficulties of learning new skills or utilizing new tools and processes.

VMware Site Recovery is an add-on feature to VMware Cloud on AWS. VMware Cloud on AWS integrates VMware's flagship compute, storage, and network virtualization products: VMware vSphere, VMware vSAN, and VMware NSX along with VMware vCenter Server management. It optimizes them to run on elastic, bare-metal AWS infrastructure. With the same architecture and operational experience on-premises and in the cloud, IT teams can now get instant business value via the AWS and VMware hybrid cloud experience.

The VMware Cloud on AWS solution enables customers to have the flexibility to treat their private cloud and public cloud as equal partners and to easily transfer workloads between them, for example, to move applications from DevTest to production or burst capacity. Users can leverage the global AWS footprint while getting the benefits of elastically scalable SDDC clusters, a single bill from VMware for its tightly integrated software plus AWS infrastructure, and on-demand or subscription services like VMware Site Recovery Service. VMware Site Recovery extends VMware Cloud on AWS to provide a managed disaster recovery, disaster avoidance and non-disruptive testing capabilities to VMware customers without the need for a secondary site, or complex configuration.

VMware Site Recovery works in conjunction with VMware Site Recovery Manager and VMware vSphere Replication to automate the process of recovering, testing, re-protecting, and failing-back virtual machine workloads. VMware Site Recovery utilizes VMware Site Recovery Manager servers to coordinate the operations of the VMware SDDC. This is so that, as virtual machines at the protected site are shut down, copies of these virtual machines at the recovery site startup. By using the data replicated from the protected site these virtual machines assume responsibility for providing the same services.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM5.jpg)

VMware Site Recovery can be used between a customers datacenter and an SDDC deployed on VMware Cloud on AWS or it can be used between two SDDCs deployed to different AWS availability zones or regions. The second option allows VMware Site Recovery to provide a fully VMware managed and maintained Disaster Recovery solution. Migration of protected inventory and services from one site to the other is controlled by a recovery plan that specifies the order in which virtual machines are shut down and started up, the resource pools to which they are allocated, and the networks they can access.

VMware Site Recovery enables the testing of recovery plans, using a temporary copy of the replicated data, and isolated networks in a way that does not disrupt ongoing operations at either site. Multiple recovery plans can be configured to migrate individual applications or entire sites providing finer control over what virtual machines are failed over and failed back. This also enables flexible testing schedules. VMware Site Recovery extends the feature set of the virtual infrastructure platform to provide for rapid business continuity through partial or complete site failures.
-->

## VMware Site Recovery 

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM4.jpg)

VMware Site Recovery は、VMware のエンタープライズ クラスの ソフトウェア定義データセンター (SDDC) における災害対策を、サービスとして AWS クラウドに取り込みました。VMware Site Recovery により、専用のセカンダリ サイトを必要とせず、アプリケーションの保護、復旧が可能となります。VMware Site Recovery は、オンデマンド サービスとして VMware により提供、販売、サポート、維持および運用されます。IT チームは、クラウド ベースのリソースを使い慣れた VMware のツールを用いて管理することができます。これにより新しいスキルを学習したり、新しいツールやプロセスを適用する困難から解放されます。

VMware Site Recovery は VMware Cloud on AWS のアドオン機能となります。VMware Cloud on AWS は VMware のフラッグシップのコンピュート、ストレージ、そしてネットワーク仮想化製品を統合しています。ここには VMware vSphere、VMware vSAN、VMware NSX が含まれ VMware vCenter Server の管理にまとめられています。これらは、エラスティックかつベアメタルの AWS インフラストラクチャで実行されることに最適化されています。オンプレミスとクラウドで同じアーキテクチャ、同じ運用を行えるようになり、AWS と VMware のハイブリッド クラウド エクスペリエンスを通じで IT チームは即座にビジネスの価値を得ることができます。

VMware Cloud on AWS ソリューションは、顧客が柔軟性を持つことを可能とします。この柔軟性はプライベート クラウドとパブリック クラウドを同等に扱えること、簡単にワークロードを両者のクラウド間で移動できることによります。例えば、アプリケーションをテスト開発環境から本番環境に移動させることや、バースト キャパシティなどが考えられます。また顧客は、グローバルの AWS のフットプリントを活用できます。ここでは、伸縮自在の SDDC の利点を得る、高度に統合されたソフトウェアと AWS インフラストラクチャおよび VMware Site Recovery サービスといったオンデマンド サービスまたはサブスクリプション サービスの VMware からの単一の請求の利点を得るといったことが挙げられます。VMware Site Recovery は VMware Cloud on AWS を拡張し、マネージドの災害対策、災害回避および非破壊の災害対策テストを顧客に提供します。ここではセカンダリ サイトや複雑な構成などは存在しません。

VMware Site Recovery は VMware Site Recovery Manager と VMware vSphere Replication と連動して動作し、仮想マシン ワークロードの復旧、テスト、再保護、フェイルバックのプロセスを自動化します。VMware Site Recovery は VMware Site Recovery Manager サーバーを VMware SDDC の運用と連携させるために利用します。これはつまり、保護サイトの仮想マシンがシャットダウンされ、復旧サイトのコピーされた仮想マシンの起動となります。保護サイトからデータ レプリケーションされることで、これらの仮想マシンは同じサービスを提供できると考えられるのです。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM5.jpg)

VMware Site Recovery は、顧客のデータセンターと VMware Cloud on AWS にデプロイされた SDDC の間で用いられます。また、異なる AWS のアベイラビリティ ゾーンにデプロイされた SDDC 同士、あるいは、異なる AWS のリージョンにデプロイされた SDDC 同士で用いることができます。2 つ目の選択肢は、VMware Site Recovery が完全に VMware によって運用管理される災害対策のソリューションとなります。保護されたインベントリとサービスのサイト間の移行は、リカバリ プランによって制御されます。リカバリ プランでは、どの仮想マシンをどの順番でシャットダウンし起動させるか、どのリソースプールを仮想マシンに割り当てるか、仮想マシンがアクセスできるネットワークを明示します。

VMware Site Recovery では、リカバリ プランのテストを行うことができます。このテストでは、レプリケーション データの一時的なコピーと分離されたネットワークが用いられ、両サイトの稼働している運用を止めることなくテストが実施されます。どの仮想マシンをフェイルオーバーやフェイルバックさせるかを細かく制御して、個別のアプリケーションあるいはサイト全体を移行するために複数のリカバリ プランを構成することができます。この機能は柔軟なテスト スケジュールを可能にします。VMware Site Recovery は仮想インフラストラクチャ プラットフォームの機能を、サイト全体の障害、あるいは、サイトの一部の障害に対し迅速なビジネス継続性を備えるよう拡張します。

<!--
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
-->

## SDDC 間の VPN の作成

受講者の VPC とペアとなった受講者の VPC の間で IPSEC VPN を作成します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM6.jpg)

1\. *VMware Cloud on AWS* タブに戻ります

2\. SDDC ウィンドウで、*View Details* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM7.jpg)

3\. *Network* メニューをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM8.jpg)

Management Gateway セクションにて、*Public IP* と *Infrastructure Subnet CIDR* をメモします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM9.jpg)

Management Gateway の設定にて

4\. *IPsec VPNs* セクションを開くためドロップ ダウン矢印をクリックします

5\. *ADD VPN* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM10.jpg)

以下の情報を入力します

6\. Name: Student # MGMT GW (# は受講者番号)

7\. *Public IP address* はペアを組んだ受講者の Gateway の Public IP アドレスを入力

8\. *Infrastructure IP CIDR* はペアを組んだ受講者の Infrastructure Subnet CIDR を入力

9\. Pre-shared key は **VMware1!** を入力

10\. *Save* をクリック

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM11.jpg)

受講者とペアの受講者がこれらのステップを終えると、VPN のステータスが **Connected** となるはずです

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM12.jpg)

この設定を動作させるために、我々のホストインフラストラクチャに 2 つ目の VPN を設定する必要があります。これは、オンプレミスの環境で設定する際は必要ありませんが、このワークショップを実施するために必要な特別な設定となります

11\. Management Gateway にて IPSec VPN のドロップ ダウンが開かれていることを確認します

12\. *Add VPN* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM13.jpg)

以下の情報を入力します

13\. VPN の名前を **Student# to Host** (# は受講者番号)

14\. Remote Gateway Public IP に **54.70.191.234** を入力します

15\. Remote Networks に **192.168.30.0/24** を入力します

16\. Pre-shared key を **VMware1!** と入力します

17\. *Save* をクリックします

<!--
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
-->

## Site Recovery のペアリングの準備

### Site Recovery のファイアウォール ルール

このモジュールでは、VMware Cloud on AWS のポータルに新しく導入された *Firewall Rule Accelerator* オプションを利用します

Firewall rule accelerator は、ユースケースに合わせたファイアウォール ルールのグループを作成します。選択された VPN のリモート ネットワークが、これらのルールの送信元あるいは送信先となります。作成されたルールを編集することは必要ありませんが、もし必要ならば作成後に それらのルールを編集することができます

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM14.jpg)

1\. *Network* タブをクリックします

2\. *Firewall Rule Accelerator* エリアを拡げます

3\. Role Group にて *Site Recovery* オプションを選択します

4\. VPN にて、ペアとなる受講者のネットワークに対する VPN を選択します。これを Host Infrastructure に対してもこのステップを繰り返して下さい

    このステップを両方の VPN に対して行うようにしてください。1 つはペアの受講者との VPN、もう一つは Host Infrastructure への VPN となります

5\. *Create Firewall Rules* をクリックします

ファイアウォール ルールが自動的に生成されることを確認してください。完了したならば、2 つ目の VPN (Host Infrastructure) に対しても繰り返して下さい。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM15.jpg)

<!--
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
-->

### VMware Site Recovery - サイト ペアリング

*最重要*: ペアを組んだ受講者のうち、片方の受講者のみがサイト ペアリングの演習を行うことができます。あなたとペアの受講者のうち、どちらがこのステップを行うかお決め下さい

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM16.jpg)

1\. VMware Cloud on AWS のポータルにて *Add Ons* タブをクリックします

2\. *Open Site Recovery* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM17.jpg)

3\. *New Site Pair* をクリックします

インストラクターに割り当てられたパートナーのサイトとペアリングを行います。ここまで使ってきたあなたの SDDC の情報ではないことにご注意下さい

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM18.jpg)

これは、あなたのパートナーがあなたから必要とする情報、あるいは、あなたがあなたのパートナーから必要とする情報です

4\. SDDC にて *Settings* タブをクリックします

あなたのサイト、および、ペアとなるサイトのいずれのユーザー名は、いつも *cloudadmin@vmc.local* となります

5\. vCenter Server ユーザーのパスワードをコピー、もしくはメモします

6\. vCenter Server の URL をメモします。入力に使われる値と URL の違いは以下になります

*DISPLAYED*: https://vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com/ui

*USED*: vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM19.jpg)

7\. あなたのローカルの vCenter が選択されていることを確認します

8\. パートナーの SDDC の情報を入力します

  PSC host name (上記でメモされたように、正しいフォーマットでの入力を確認します)

  User name

  Password

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM20.jpg)

9\. ローカルの vCenter server が選択されていることを確認します

10\. 全てのサービスを選択します

11\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM21.jpg)

12\. *Finish* ボタンをクリックします

<!--
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
-->

### ネットワーク マッピング

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM22.jpg)

13\. Site Recovery ページ左側の *Network Mappings* をクリックします

14\. *New* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM23.jpg)

15\. *Prepare mappings manually* を選択します

16\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM24.jpg)

17\. 両サイトの *SDDC Datacenter* を拡げます

18\. 両サイトの *Management Networks* を拡げます

19\. 両サイトの *vmc-dvs* を拡げます

20\. Select your *Student#-LN* network and your partner's *Student#-LN* (You may need to scroll down to fid these networks)
あなたの *Student#-LN* ネットワークと、貴方のパートナーの *Student#-LN* を選択します

21\. *Add Mappings* ボタンをクリックします

22\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM25.jpg)

23\. Reverse Mappings では、いずれも入力、選択しないでください。*Next* をクリックします

24\. デフォルトの値のままに *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM27.jpg)

25\. *Finish* をクリックします

<!--
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
-->

### フォルダー マッピング

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM28.jpg)

26\. 画面左の *Folder Mappings* を選択します

27\. *+ New* をクリックし、新しいフォルダー マッピングを作成します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM29.jpg)

28\. *Prepare mappings manually* を選択します

29\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM30.jpg)

30\. 両サイトで *SDDC Datacenter* を拡げます 

31\. 両サイトで *Workloads* を選択します

32\. *Add Mappings* をクリックします

33\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM31.jpg)

34\. Reverse mappings は選択せず、*Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM32.jpg)

35\. *Finish* をクリックします

<!--
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
-->

### リソース マッピング

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM33.jpg)

36\. 画面左の *Resource Mappings* を選択します

37\. *+ New* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM34.jpg)

38\. 両サイトの *SDDC Datacenter* を拡げます

39\. 両サイトの *Cluster 1* を拡げます

40\. 両サイトの *Compute-ResourcePool* を選択します

41\. *Add Mappings* ボタンをクリックします

42\. *Next* をクリックします


![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM35.jpg)

43\. Reverse Mappings を選択せずに、*Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM36.jpg)

44\. *Finish* をクリックします

<!--
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
-->

### ストレージ ポリシー マッピング

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM37.jpg)

45\. 画面左の *Storage Policy Mappings* を選択します

46\. *+ New* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM38.jpg)

47\. *Prepare mappings manually* を選択します

48\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM39.jpg)

49\. 画面右側、左側の両方で *Datastore Default* を選択します

50\. *ADD MAPPINGS* をクリックします

51\. *NEXT* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM40.jpg)

52\. Reverse Mappings 向けに *Datastore Default* をクリックします

53\. *NEXT* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM41.jpg)

54\. *FINISH* をクリックします

<!--
### Placeholder Datastores

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM42.jpg)

55\. Select *Placeholder Datastores* in the left pane

56\. Click *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM43.jpg)

57\. Select *WorkloadDatastore*

58\. Click *Add*
-->

### プレースホルダー データストア

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM42.jpg)

55\. 画面左にて *Placeholder Datastores* を選択します

56\. *+ New* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM43.jpg)

57\. *WorkloadDatastore* を選択します

58\. *Add* をクリックします

<!--
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
-->

## SRM - 仮想マシンの保護

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM44.jpg)

1\. 仮想マシンを選択し、右クリックします

2\. *All Site Recovery actions* を選択します

3\. *Configure Replication* をクリックします

*注意*: ペアとなるサイト (あなたのパートナーのサイト) にログインする必要があります。*cloudadmin@vmc.local* ユーザーとパートナーのパスワードを使っていることを確認します。入力後、もう一度このステップを繰り返す必要があるかも知れません。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM45.jpg)

4\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM46.jpg)

5\. ターゲット サイトを選択します

6\. まだログインしていなければ、ここでログインします (あなたのパートナーのパスワードを使うことを忘れないで下さい)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM47.jpg)

7\. あなたのパートナーのサイトのクレデンシャルを入力します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM48.jpg)

8\. すべてデフォルトの値のままとし、*Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM49.jpg)

9\. VM Storage Policy はデフォルトの値 *Datastore Default* のままとします

10\. *WorkloadDatastore* を選択します

11\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM50.jpg)

12\. Recovery Point Objective をデフォルトの 1 時間とします。RPO は最小 5 分、最大 24 時間に設定することができます

13\. *Next* をクリック

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM51.jpg)

14\. *Add to new protection group* を選択します

15\. Protection Group 名を *PG#* とします (# は受講者番号)

16\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM52.jpg)

17\. *Add to new recovery plan* を選択します

18\. Recovery Plan 名を **RP#** とします (# は受講者番号)

19\. *Next* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM53.jpg)

20\. *Finish* をクリックします

<!--
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
-->

## リカバリ テストの実施

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM54.jpg)

1\. VMware Cloud on AWS のポータルにて *Add Ons* タブをクリックします

2\. *Open Site Recovery* をクリックします (ブラウザのポップアップを許可する必要があるかも知れません)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM55.jpg)

3\. Site Recovery ウィンドウにて、*Open* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM56.jpg)

4\. *Recovery Plans* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM57.jpg)

5\. Protection Group *PG#* をクリックします (# は受講者番号)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM58.jpg)

6\. *Recovery Plans* をクリックします

7\. 前のステップで作成した Recovery Plan *RP#* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM59.jpg)

8\. *Test* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM60.jpg)

9\. 全てをデフォルトの値のままとし、*Next* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM61.jpg)

10\. *Finish* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM62.jpg)

11\. ウィンドウ下部の Recent Tasks エリアの進捗を追って下さい

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM63.jpg)

12\. テストが正常に完了したことを確認してください

13\. ここまでのアクティビティをクリーンアップし、元の通常の状態に環境を戻すため、*Cleanup* ボタンをクリックしてください

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM64.jpg)

14\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM65.jpg)

15\. *Finish* をクリックします。ここで環境はクリーンアップされます。テストを行っている間も、仮想マシンを保護するレプリケーションは停止されることはありません。