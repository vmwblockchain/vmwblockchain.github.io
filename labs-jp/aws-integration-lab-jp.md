---
layout: single
title: "AWS Integration ラボ マニュアル"
categories: labs
date: 2018-08-29
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
<!-- based on commit: 42111cae814573ce6f6ffe9ec57f08673c9fbe0e -->

<!--
# Introduction

One of the most compelling reasons to adopt VMware Cloud on AWS is to integrate your existing systems which sit in your VMware cloud environment, with application platforms which reside in your AWS Virtual Private Cloud (VPC) environment. The intergration which VMware and AWS have created allows for these services to communicate, for free, across a private network address space for services such as EC2 instances, which connect into subnets within a native AWS VPC, or with platform services which have the ability to connect to a VPC Endpoint, such as S3 Storage.

In this lab we will work through some common basic integrations which you can utilise in your VMware Cloud on AWS platform.

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs-jp/working-with-sddc-lab-jp/) concerning Content Library creation and Network creation and firewall rule creation.
-->

# はじめに

VMware Cloud on AWS を採用する時の最も切実な理由の一つに、VMware Cloud on AWS 環境にある既存のシステムを、AWS Virtual Private Cloud (VPC) にあるアプリケーション プラットフォームと統合することが挙げられます。VMware と AWS によるインテグレーションは、既存のシステムと、EC2 のようなサービスのためのプライベート ネットワーク アドレス空間の通信を可能とします。また S3 ストレージのような VPC エンドポイントによる接続が可能なプラットフォーム サービスとも通信が可能となります。そしてこれらの通信は無料です。

このラボでは、VMware Cloud on AWS のプラットフォームでも利用できるいくつかの良くある基本的なインテグレーションに取り組みます。

注意: このラボの前提として [SDDC Lab を使ってみる](https://vmc-field-team.github.io/labs-jp/working-with-sddc-lab-jp/) の、コンテント ライブラリの作成、ネットワークの作成、およびファイアウォール ルールの作成を完了して下さい。

<!--
## Integration with AWS Simple Storage Service (S3)

### AWS S3 Backed vSphere Content Library

vSphere Content Libraries enable customers to take advantage of different types of storage backing than just vSphere Datastores. The primary requirement is that the content endpoint is accessible over HTTP(s), which means that a number of solutions can be used from a simple web server like Nginx to an advanced distributed object store like AWS's Simple Storage Service (S3).

The work ow to create a 3rd Party vSphere Content Library on S3 is as follows:

1\. Upload and organize the content on S3

2\. Run a python script to index and generate the Content Library metadata

3\. With the ability to remotely index and generate the Content Library metadata files, you do
not have to keep a local copy of all your content. All changes can be made directly on the S3 bucket and then simply re-run the script to generate the updated metadata which can even be scheduled as a simple cron job.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-1.jpg)

The python script referenced which can be downloaded [here](https://code.vmware.com/samples/4388/automating-the-creation-of-3rd-party-content-library-directly-on-amazon-s3) can now index content both locally as well as a remote S3 bucket.

In case you did not know, S3 usage (ingress/egress) from a customers SDDC is 100% free for VMware Cloud on AWS customers by simply using a linked S3 Endpoint. This means you can take advantage of S3 to store your templates, ISOs and other static files, which can also be shared by other SDDCs. This means you are not consuming any of your primary storage for static content and can be used for what it was meant, for your workloads.

For the purpose of this exercise, and in the interest of time, the contents of this exercise have been uploaded to an existing S3 bucket in AWS for you. Let's create the S3 backed Content Library!
-->

## AWS Simple Storage Service (S3) との連携

### AWS S3 を使った vSphere コンテンツ ライブラリ

vSphere コンテンツ ライブラリは、vSphere データストアだけでなく、異なるタイプのストレージ バッキングを利用することができます。主な要件は、コンテンツのエンドポイントが HTTP(S) としてアクセス可能なことです。つまり、Nginx のようなシンプルな Web サーバーから、AWS Simple Storage Service (S3) のような高度な分散オブジェクト ストアまで多くのソリューションを使うことができるのです

S3 上に 3rd パーティーの vSphere コンテンツ ライブラリを作成する手順は以下になります

1\. コンテンツを S3 にアップロードし正しく配置します

2\. コンテンツ ライブラリのメタデータをインデックス化して生成するために Python スクリプトを実行します

3\. リモートにコンテンツ ライブラリのメタデータ ファイルをインデックス化して生成できたなら、全てのコンテンツをローカルに保持する必要はありません。全ての変更は S3 バケットで行うことができ、そして最新のメタデータを生成するためのスクリプトを単純に再実行するだけです。このスクリプトは cron ジョブとしてスケジュールすることさえできます

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-1.jpg)

[こちら](https://code.vmware.com/samples/4388/automating-the-creation-of-3rd-party-content-library-directly-on-amazon-s3) からダウンロードできる python スクリプトは、ローカルだけでなくリモートの S3 バケットのコンテンツもインデックス化できます

カスタマーの SDDC からの S3 の Igress/Engress のトラフィックは、リンクされた S3 エンドポイントを使うことで VMware Cloud on AWS のカスタマーには 100% 無料となります。これにより、テンプレート、ISO、その他の性的なファイルを保管するのに S3 を利用することができ、それらを他の SDDC から共有することもできます。よって、プライマリ ストレージを、静的なコンテンツに一切消費することなく、ワークロードに使うことができます

この演習では時短のため、演習のコンテンツは AWS の既存の S3 バケットにアップロードされています。それでは S3 を使ったコンテンツ ライブラリを作成しましょう！

<!--
### Add S3 Backed Content Library

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-2.jpg)

1\. On your VMware Cloud on AWS vCenter window click on *Menu*

2\. Click on *Content Libraries*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-3.jpg)

3\. In your Content Library window, click the *+* sign to add a new Content Library.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-4.jpg)

4\. Name your Content Library *S3 Content Library* or any other name of your choosing

5\. (Optional) Enter some notes for your Content Library

6\. Click *NEXT* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-5.jpg)

7\. Select *Subscribed content library*

8\. Under *Subscription URL* enter the following: **http://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json**

9\. Make sure Download content is set to *immediately*

10\. Click *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-6.jpg)

11\. On the *Unable to very identity of the subscription host* notification click *YES*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-7.jpg)

12\. Highlight the *WorkloadDatastore* as the storage location

13\. Click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-8.jpg)

14\. Click *Finish* button. Your content library should take about 20+ minutes to complete syncing.

It may take a few minutes for your Content to sync, periodically check your content library to ensure you see the content.

Congratulations, you have completed this exercise.
-->

### S3 を使ったコンテンツ ライブラリの追加

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-2.jpg)

1\. VMware Cloud on AWS の vCenter のウィンドウで *Menu* をクリックします

2\. *Content Libraries* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-3.jpg)

3\. コンテンツ ライブラリ ウィンドウで、新しいコンテンツ ライブラリを作成するために *+* マークをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-4.jpg)

4\. コンテンツ ライブラリの名前を *S3 Content Library* もしくは お好みの名前とします

5\. (オプション) コンテンツ ライブラリにメモを入力します

6\. *NEXT* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-5.jpg)

7\. *Subscribed content library* を選択します

8\. *Subscription URL* に次の値を入力します: **http://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json**

9\. Download content が *immediately* に設定されていることを確認します

10\. *NEXT* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-6.jpg)

11\. *Unable to very identity of the subscription host* という通知で *YES* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-7.jpg)

12\. ストレージ ロケーションとして *WorkloadDatastore* をハイライトさせます

13\. *Next* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-8.jpg)

14\. *Finish* ボタンをクリックします。コンテンツ ライブラリが同期完了するまで 20 分程度かかります

同期完了まで数分かかります。定期的に、コンテンツを確認できるかコンテンツ ライブラリをチェックしてください

おめでとうございます。この演習を完了しました

<!--
## AWS Relational Database Service (RDS) Integration

### Deploy Photo VM

As a first step in setting up our integration between the VMware vSphere platform in VMware Cloud on AWS and native AWS services, we are going deploy a VM which we will use for this demo. This VM will be referred to as "Photo VM". Please follow the instructions below.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

1\. If not already opened, open your VMware Cloud on AWS vCenter and click on the *Menu* drop down

2\. Select *Content Libraries*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

3\. Click on your previously created Content Library named *Student#* (where # is your student number)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

4\. Make sure you click on the *Template* tab

5\. Right-click on the *photo app* Template

6\. Select *New VM from This Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

7\. Name the virtual machine **PhotoApp#** (where # is your student #)

8\. Expand the location and select *Workloads*

9\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS5.jpg)

10\. Expand the destination to select *Compute-ResourcePool* as the compute resource

11\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

12\. In the **Review details** step click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS7.jpg)

13\. In the **Select storage** step, highlight the *WorkloadDatastore*

14\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS8.jpg)

15\. Select the network you created in a previous step for your VM

16\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

17\. Click *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

18\. Monitor the deployment of your VM until it's completed

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

19\. Check for completion of the deployment of your VM

20\. Click *Menu*

21\. Select *VMs and Templates*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

22\. Check to make sure your VM is powered on. If not, right-click on your VM

23\. Select *Power -> **Power On*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

24\. Make sure your VM is assigned an IP addresses (may need to a few minutes for this information to be populated). Make a note of this IP address for a future step.

-->

## AWS リレーショナル データベース サービス (RDS) インテグレーション

### Photo VM のデプロイ

VMware Cloud on AWS の VMware vSphere プラットフォームとネイティブの AWS サービスのインテグレーションをセットアップする最初のステップとして、このデモで使う仮想マシンをデプロイします。この VM は Photo VM として参照されます。以下の手順を進めて下さい。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

1\. もし開いていないのであれば、VMware Cloud on AWS の vCenter を開き、**Menu** ドロップ ダウンをクリックしてください

2\. **Content Libraries** を選択してください

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

3\. **Student#** と名前を付けて前に作成したコンテント ライブラリをクリックします (# は受講者番号)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

4\. **Template** タブがクリックされていることを確認してください

5\. **photo app** テンプレートを右クリックします

6\. **New VM from This Template** を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

7\. 仮想マシンの名前を **PhotoApp#** とします (# は受講者番号)

8\. ツリーを展開し、**Workloads** を選択します

9\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS5.jpg)

10\. ツリーを展開し **Compute-ResourcePool** をコンピュート リソースとして選択します

11\. **Next** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

12\. "詳細の確認" ステップで **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS7.jpg)

13\. **Select Storage** ステップで、"WorkloadDatastore" をハイライトします

14\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS8.jpg)

15\. 仮想マシンのために前に作成したネットワークを選択します

16\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

17\. **FINISH** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

18\. デプロイが完了するまで、デプロイメント タスクを監視します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

19\. 仮想マシンのデプロイが完了したことを確認します

20\. **Menu** をクリックします

21\. **VMs and Templates** を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

22\. デプロイされた仮想マシンで右クリックし、仮想マシンをパワーオンします

23\. **Power** -> **Power On** を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

24\. 仮想マシンに IP アドレスが付与されていることを確認します (この情報が反映されるまで数分かかるかもしれません)。この後のステップのために、この IP アドレスをメモしておいてください

<!--
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS14.jpg)

25\. Go back your VMware Cloud on AWS portal and click on the *Network* tab in order to request a Public IP address

26\. Under the *Compute Gateway* click and expand *Public IPs*

27\. Click on *REQUEST PUBLIC IP**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)

28\. (Optional) Enter Notes for this public IP, such as the name of the VM we are linking this too

29\. Click on *Request*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

30\. You should see a similar notification as the one above

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS17.jpg)

31\. Take note of your newly acquired Public IP address

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS18.jpg)

32\. Next you will create a NAT rule from the newly acquired Public IP address you noted in your last step to the internal IP address of the VM you created. Click on *NAT* under the *Compute Gateway* section to expand the NAT Rules

33\. Click *ADD NAT RULE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS19.jpg)

34\. Give your NAT rule a name

35\. Your new Public IP address should be pre-filled for you, if not, select it now

36\. Under *Service* select *Any (All Traffic)*

37\. Type your VM's internal IP address

38\. Click the *SAVE* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS20.jpg)

39\. You should get a *NAT rule successfully created* notification

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS21.jpg)

40\. Expand *Firewall Rules* under the Compute Gateway section

41\. Click *ADD RULE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS22.jpg)

42\. Give your Firewall Rule a name

43\. Select *All Internet and VPN* for Source

44\. Type the Public IP Address you noted under *Destination*

45\. Select *Any (All Traffic)* for *Service*

46\. Click *SAVE* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS23.jpg)

47\. You should get a *Firewall rule successfully created* notification
-->

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS14.jpg)

25\. VMware Cloud on AWS のポータルに戻り、**Public IP Address** をリクエストするために **Network** タブをクリックします

26\. **Compute Gateway** セクションの **Public IP** をクリックし展開します

27\. **REQUEST PUBLIC IP** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)

28\. (オプション) この Public IP にリンクさせる仮想マシンの名前など、この Public IP のための注記を入力します

29\. **Request** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

30\.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS17.jpg)

31\. 新しく取得された Public IP アドレスをメモします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS18.jpg)

32\. 次に、前のステップでメモした新しく取得された Public IP アドレスから作成された仮想マシンの内部 IP アドレスへの **NAT Rule** を作成します。**Compute Gateway** セクションの **NAT** をクリックし NAT ルールを展開します

33\. **ADD NAT RULE** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS19.jpg)

34\. **NAT rule** に名前を入力します

35\. 新しい Public IP が既に入力済みのはずですが、そうでない場合は ここで選択します

36\. **Service** の下で **Any (All Traffic)** を選択します

37\. 仮想マシンの内部 IP アドレスを入力します

38\. **SAVE** ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS20.jpg)

39\. **NAT rule successfully created** という通知を確認します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS21.jpg)

40\. **Compute Gateway** セクションの **Firewall Rules** を展開します

41\. **ADD RULE** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS22.jpg)

42\. ファイアウォール ルールの名前を入力します

43\. **Source** では **All Internet and VPN** を選択します

44\. **Destination** にメモした Public IP アドレスを入力します

45\. **Any (All Traffic)** for **Service** では **Any (All Traffic)** を選択します

46\. **SAVE** ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS23.jpg)

47\. **Firewall rule successfully created** という通知を確認します

<!--
### AWS Relational Database Service (RDS Configuration)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS24.jpg)

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

48\. For Account ID or alias ensure *vmcworkshop* is specified

49\. IAM user name - **Student#** (where # is the number assigned to you)

50\. Password - **VMCworkshop1211**

51\. Click *Sign In*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS25.jpg)

52\. You are now signed in to the AWS console. Make sure the region selected is *Oregon* in the top right hand corner of the AWS Console

53\. Click on the *RDS* service under the "Database" section

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS26.jpg)

54\. In the left pane click on *Instances*

55\. Click on the RDS instance that corresponds to your Student number

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS27.jpg)

56\. Scroll down to the *Details* area and under *Security and network* notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS28.jpg)

57\. Go back to the main *Services* page in the AWS console by clicking the *Services* link in the top left corner of the console

58\. Scroll down to *Networking & Content Delivery* and click *VPC*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS29.jpg)

59\. Click on *Security Groups* in the left pane under the "Security" section

60\. Choose the **rds-launch-wizard-#** RDS Security group corresponding to your student number (where # is the number assigned to you)

61\. After highlighting the appropriate security group click on the *Inbound Rules* tab below.

VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own.

62\. Notice that the CIDR block range of your Student#-LN Logical Network you created in VMware Cloud on AWS is authorized for MySQL on port 3306. This was done for you ahead of time

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS30.jpg)

AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.

63\. Click on *Services* in the top left of the console to go back to the Main Console

64\. Click on *EC2* under the Compute section

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS31.jpg)

65\. In the EC2 Dashboard click *Network Interfaces* in the left pane under the *Network Interfaces* section

66\. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view, type *RDS* in the filter area and press *Enter* to add a filter

67\. Highlight your **rds-launch-wizard-#** corresponding to your student number (where # is the number assigned to you)

68\. Make note of the *Primary private IPv4* IP address for the next step

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS32.jpg)

69\. Open an additional browser tab and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: x.x.x.x/Lychee

70\. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:
    Database Host: **x.x.x.x:3306**
    Database Username: **student#** (where # is the number assigned to you)
    Database Password: **VMware1!**

71\. Click *Connect*

You have now successfully created a Hybrid Application utilises components across both your VMware on AWS SDDC environment and your AWS services.

This functionality provides customers now with choices around how applications are migrated to the cloud. You can now split your application across platforms and consume services in vSphere and AWS as it makes sense. This level of choice can really help those who are looking to migrate to the cloud, accelerate that process by not getting "bogged down" in refactoring parts of an application which are difficult to move to hyperscale cloud.
-->

### AWS リレーショナル データベース サービス (RDS の構成)

ブラウザで 新しいタブを開き、<https://vmcworkshop.signin.aws.amazon.com/console> を開きます

48\. アカウント ID またはエイリアスに、"vmcworkshop" と入力されていることを確認します

49\. IAM ユーザー名 - Student# (# は受講者番号)

50\. パスワード - VMCworkshop1211

51\. **サインイン** をクリック

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS25.jpg)

52\. これで AWS コンソールにログインしました。AWS コンソールの右上角で、リージョンが **オレゴン** と選択されていることを確認します

53\. AWS コンソール上部の **サービス** をクリックし、"データベース" セクションの **RDS** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS26.jpg)

54\. 左ペインの **インスタンス** をクリックします

55\. 受講者番号に対応する RDS インスタンスをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS27.jpg)

56\. **詳細** エリアまでスクロールさせ、**セキュリティとネットワーク** の下で RDS インスタンスがパブリックアクセス可能ではないことを確
認します。これは このインスタンスが AWS からのみアクセスできることを意味します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS28.jpg)


57\. AWS コンソールの左上角の **サービス** をクリックし、AWS コンソールのメイン サービス ページに戻ります

58\. **ネットワーキング & コンテンツ配信** へスクロールし **VPC** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS29.jpg)

59\. 左ペインの "セキュリティ" セクションの下の **セキュリティ グループ** をクリックします

60\. 受講者番号に対応した RDS のセキュリティ グループ **rds-launch-wizard-#** を選択します (# は受講者番号)

61\. 適切なセキュリティ グループをハイライトした後、下部の **インバウンド ルール** をクリックします。

<!-- 文意が汲み取れず: VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own. -->

62\. VMware Cloud on AWS で作成した Student#-LN 論理スイッチの CIDR ブロック レンジが MySQL のポート 3306 へ許可されていることを確認します。これは事前に設定されています

リレーショナル データベース サービス (RDS) は、VMware Cloud on AWS によって作成される ENI とは別に、アクセスのための自身の Elastic Network Interface (ENI) を作成します。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS30.jpg)

63\. コンソールの左上の **サービス** をクリックし、メイン コンソールに戻ります

64\. コンピューティング セクションの下の **EC2** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS31.jpg)

65\. EC2 のダッシュボードにて、"ネットワーク & セキュリティ" セクションの下の **ネットワーク インターフェイス** をクリックします

66\. 全ての受講者の環境は同じ AWS アカウントに属しているため、数百の ENI があるかもしれません。ビューを小さくするために、フィルター エリアに "RDS" と入力し、そのフィルターを追加するために **Enter** キーを押します

67\. 受講者番号に対応した **rds-launch-wizard-#** セキュリティ グループを持つ ENI をハイライトします。(# は受講者番号)

68\. 次のステップのために **プライマリ プライベート IPv4** の IP アドレスをメモします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS32.jpg)

69\. 新しいブラウザ タブを開き、ブラウザのアドレスバーに VMware Cloud on AWS でリクエストした IP アドレスを入力し、続いて /Lychee (大文字と小文字を区別) と入力します。例) x.x.x.x/Lychee

70\. 以下のデータベース接続情報を入力します。先ほどメモした RDS の ENI の IP アドレスを利用します:

    - **Database Host**: x.x.x.x:3306
    - **Database Username**: student# (# は受講者番号)
    - **Database Password**:VMware1!

71\. **Connect** をクリックします

これで、VMware Cloud on AWS の SDDC 環境と AWS のサービスの両方に跨がったコンポーネントを利用するハイブリッド アプリケーションの構築に成功しました。

この機能により、顧客はアプリケーションをクラウドに移行する方法を選択できるようになります。アプリケーションをプラットフォームを跨いで分割し、vSphere と AWS のサービスを利用することができます。このレベルの選択肢はクラウドへ移行しようとしている人々の助けとなります。なぜならば、クラウド レベルのハイパースケールに対応させるのが困難なアプリケーションのリファクタリングに行き詰まることなく移行できるからです。

<!--
## AWS Elastic File System (EFS) Integration

### EFS VM Creation

In our next section on integrating AWS services with VMware Cloud on AWS. We will utilise the AWS Elastic File System (EFS) which we can use to mount NFS shares to our VMware Cloud on AWS hosted virtual machines.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS1.jpg)

1\. Navigate to your Content Library, click *Menu* on your VMware Cloud on AWS vCenter Server

2\. Select *Content Libraries*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS2.jpg)

3\. Click on your *Student#* Content Library

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS3.jpg)

4\. Make sure the *Templates* tab is selected

5\. Right-click on the *efs* template

6\. Select *New VM from This Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS4.jpg)

7\. Name your VM **EFSVM#** (where # is your student number)

8\. Select *Workloads* for the location of your VM

9\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS5.jpg)

10\. Select *Compute-ResourcePool* as the destination for your VM

11\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS6.jpg)

12\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS7.jpg)

13\. Select *WorkloadDatastore* for storage

14\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS8.jpg)

15\. Select your Destination Network.  This should be *Student#-LN*

16\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS9.jpg)

17\. Click *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS10.jpg)

18\. Make sure to Power on your VM and ensure it has an assigned private IP address
-->

## AWS Elastic File System (EFS) インテグレーション

### EFS VM の作成

このセクションの AWS と VMware Cloud on AWS のインテグレーションでは、VMware Cloud on AWS にホストされた仮想マシンから NFS 共有を使うことができる AWS Elastic File System (EFS) を利用します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS1.jpg)

1\. コンテンツ ライブラリにナビゲートするために、VMware Cloud on AWS の vCenter で **メニュー** をクリックします

2\. **コンテンツ ライブラリ** を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS2.jpg)

3\. コンテンツ ライブラリ **Student#** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS3.jpg)

4\. **テンプレート** タブが選択されていることを確認します

5\. **efs** テンプレートの上で右クリックします

6\. **このテンプレートから仮想マシンを新規作成** を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS4.jpg)

7\. 仮想マシンの名前を **EFSVM#** とします (# は受講者番号)

8\. ツリーを展開し **Workloads** を選択します

9\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS5.jpg)

10\. ツリーを展開し **Compute-ResourcePool** をコンピュート リソースとして選択します

11\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS6.jpg)

12\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS7.jpg)

13\. ストレージは **WorkloadDatastore** を選択します

14\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS8.jpg)

15\. ネットワークを選択します

16\. **NEXT** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS9.jpg)

17\. **FINISH** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS10.jpg)

18\. 仮想マシンがパワーオンされるのを確認し、プライベート IP アドレスが割り当てられるのを確認します

<!--
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS11.jpg)

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

19\. Account ID or alias - **vmcworkshop**

20\. IAM user name - **Student#** (where # is the number assigned to you)

21\. Password - **VMCworkshop1211**

22\. Click *Sign In*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS12.jpg)

23\. You are now signed in to the AWS console. Make sure the region selected is *Oregon*

24\. Click on the *EFS* service under the storage section

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS13.jpg)

25\. Select your Student# NFS (where # is the number assigned to you)

26\. Note the IP address

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS14.jpg)

27\. Back on your vCenter Server tab, click on *Launch Web Console* for your EFS VM (Might need to allow pop ups in browser). Log in using the following credentials:
  User: **root**
  Password: **VMware1!**

Enter the following commands at the prompt:

• **cd /**

• **mkdir efs**

• **sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs**

    Where MOUNT_TARGET_IP is the IP you noted for your EFS

• **cd efs**

• **touch hello.world**

• **ls**

You have now integrated AWS EFS with a VM running in VMware Cloud on AWS.
-->

### AWS Elastic File System (EFS)

ブラウザで新しいタブを開き、<https://vmcworkshop.signin.aws.amazon.com/console> を開きます

19\. アカウント ID またはエイリアスに、"vmcworkshop" と入力されていることを確認します

20\. IAM ユーザー名 - Student# (# は受講者番号)

21\. パスワード - VMCworkshop1211

22\. **サインイン** をクリック

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS12.jpg)

23\. これで AWS コンソールにログインしました。AWS コンソールの右上角で、リージョンが **オレゴン** と選択されていることを確認します

24\. ストレージ セクションの下の **EFS** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS13.jpg)

25\. Student# NFS を選択します (# は受講者番号)

26\. IP アドレスをメモします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS14.jpg)

27\. vCenter Server のタブに戻り、EFS VM の **Web コンソールの起動** をクリックします (ブラウザのポップアップ許可が必要かも知れません)。以下のクレデンシャルを使いログインします:
 a. **User**: root
 b. **Password**: VMware1!

以下のコマンドをプロンプトで実行します:

``` bash
cd /
mkdir efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs Where MOUNT_TARGET_IP is the IP you noted for your EFS
cd efs
touch hello.world
ls
```

これで AWS EFS と VMware Cloud on AWS で稼働している仮想マシンのインテグレーションが完了しました。