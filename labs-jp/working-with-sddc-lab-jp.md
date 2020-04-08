---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-10-01
tags: workshop
toc: true
classes: wide
author_profile: false
---
<!-- based on commit: 6e3bb07e960d5017ab8ea728e24f318e2cdb27ae -->

<!--
# Introduction

In this lab we are going to start with looking at the basic tasks which you will perform in the VMware Cloud on AWS user interface when you are administering the platform.
-->

# はじめに

このラボでは、VMware Cloud on AWS プラットフォームを管理する際に、VMware Cloud on AWS のユーザーインターフェースで行う基本的なタスクを見ていくことから始めます。

<!--
## Add a Host to your SDDC

We will start by adding a host to the VMware Cloud on AWS platform. For the purposes of this lab, we have already pre-created the VMware Cloud on AWS SDDC environments for you, in order to save time.

In your chrome browser you will see a bookmark on the bookmarks bar named **VMware Cloud Services**. Please click this bookmark.

You will be directed to a login page for Cloud Services from VMware. You will need to login with the email address which you signed up to the VMware Cloud on AWS Experience day with. You will also need to ensure that this email address is associated with a "MyVMware Account" in order for the login to VMware Cloud Services to work correctly.

You will now be logged into your organization hosting your VMware Cloud on AWS SDDC cluster.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC1.jpg)

1\. On your *Student Workshop #* SDDC, click on the *View Details* button.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC2.jpg)

2\. Click on the *Actions* button in the upper right hand corner of the UI

3\. Click on *Add Hosts*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC3.jpg)

4\. In this student environment, we have restricted the platform to only allow the addition of one host. If this were a customer environment there would not be this restriction

5. Click the *Add Hosts* button

Your SDDC environment will now go through the process of adding an additional host to the SDDC environment. The process can take up to 10 minutes to complete. In the background automated tasks are provisioning an additional host, adding the host to the SDDC cluster and extending the VSAN cluster to utilize the storage presented by this additional host. All of this is done without human interaction in roughly 10 minutes. You may choose to wait for this action to process or move onto the next exercise and check back later to see your additional host.

Congratulations! You have completed this step. Please move onto configuring your SDDC Firewall rules below.
-->
## SDDC へのホストの追加

VMware Cloud on AWS プラットフォームにホストを追加するところから初めて行きます。このラボのために、既に作成済みの VMware Cloud on AWS 環境をご用意しております。これは時間短縮のためです。

Chrome ブラウザのブックマーク バーには **VMware Cloud on AWS Services** というブックマークが確認できます。このブックマークをクリックしてください。

VMware から提供される Cloud Services のログイン ページが表示されます。VMware Cloud on AWS Experience day の申込に使用したメール アドレスを使ってログインする必要があります。VMware Cloud Services のログインを正常に行うために、このメール アドレスが "MyVMware アカウント" に関連づけられていることをご確認ください。

それでは VMware Cloud on AWS の SDDC クラスタのある組織にログインしてください。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC1.jpg)

1\. 受講生に割り当てられた SDDC "Student Workshop #" の "View Details" ボタンをクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC2.jpg)

2\. "Actions" をクリックします。

3\. "Add Hosts" をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC3.jpg)

4\. このラボでは、1 ホストのみしか追加できないよう制限されています。もし実際のお客様環境の場合、この制限はありません。

5\. "Add Hosts" ボタンをクリックします。

SDDC 環境は、この環境にホストを追加するプロセスにあります。このプロセスは完了するまでに 10 分程度掛かります。バックグラウンドにて自動化されたタスクは、追加のホストのプロビジョニング、そのホストの SDDC クラスタへの追加、追加されたホストのストレージを利用するための vSAN クラスタの拡張が含まれます。これらの全ては、人手での操作なしに
行われ、おおよそ 10 分程度で完了します。このアクションが完了するまで待つこともできますし、先の演習に進みあとから追加されたホストを確認することもできます。

おめでとうございます！これでこのステップは完了です。SDDC のファイアウォール ルールの構成に進んでください

----------------
<!--
## Configuring SDDC Firewall Rules

In VMware Cloud on AWS we have two Edge Gateways which are protecting the two main networks in the VMware Cloud on AWS SDDC. The **Management Network** and the **Compute Network**. When we first initiate your SDDC environment, the default is for all traffic to both the Management and Compute networks to be denied. In this exercise we will go through the steps required to open up firewall rules so that we can manage the SDDC and not only access compute workloads but allow those compute workloads to communicate with native AWS services.
-->

## SDDC のファイアウォール ルールの構成

VMware Cloud on AWS では、VMware Cloud on AWS の 2 つの主なネットワークを保護する 2 つのエッジ ゲートウェイがあります。そのネットワークは **Management Network** と **Compute Network** です。SDDC をデプロイした当初は、デフォルトでマネージメントとコンピュートのネットワークの全てのトラフィックが拒否されています。この演習では、ファイアウォール ルールを適用するのに必要なステップを見ていきます。これにより、SDDC を管理でき、コンピュート ワークロードにアクセスできるだけでなく、これらコンピュート ワークロードがネイティブの AWS サービスに通信できるようになります。

<!--
### Management Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC4.jpg)

By default, the firewall for the Management Gateway is set to deny all inbound and outbound traffic. In this exercise, you will add a firewall rule to allow vCenter traffic. This will allow you to access the vCenter Server in your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC5.jpg)

1\. Click *Network* tab

2\. Under Management Gateway expand *Firewall Rules*

3\. Click *Add Rule*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC6.jpg)

4\. Enter a name for your rule under *Rule Name*, For Example, "vCenter-Allow-Any"

5\. Type *Any* for Source

6\. Make sure *vCenter* is selected as Destination

7\. Select *HTTPS (TCP 443)* from the drop down box for Service

8\. Click the *SAVE* button, your rule should look like the below image

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC7.jpg)
-->
### マネージメント ゲートウェイのファイアウォール ルール

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC4.jpg)

デフォルトでは、マネージメント ゲートウェイのファイアウォールはインバウンド、アウトバウンドの全てのトラフィックが拒否 (Deny) に設定されています。この演習では、vCenter へのトラフィックを許可するファイアウォール ルールを追加します。この設定により SDDC の vCenter Server にアクセスできるようになります

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC5.jpg)

1\. *Network* タブをクリックします

2\. *Management Gateway* 下の *Firewall Rules* を展開します。

3\. *Add Rule* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC6.jpg)

4\. *Rule Name* の下にルールの名前を入力します。例: "vCenter-Allow-Any"

5\. Source には *Any* と入力します

6\. Destination として *vCenter* が選択されていることを確認します

7\. *HTTPS (TCP 443)* を Service のドロップ ダウン ボックスから選択します

8\. *SAVE* ボタンをクリックすると、以下のイメージのようにルールが作成されます

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC7.jpg)

<!--
### Compute Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC8.jpg)

Like the Management NSX Edge Services Gateway. By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow access to your workload VMs which you provision in the VMware Cloud on AWS platform.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC9.jpg)

Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access

1\. Under *Network* tab, navigate to *Compute Gateway*

2\. Expand *Firewall Rules*

3\. Click *ADD RULE*
-->

### コンピュート ゲートウェイのファイアウォール

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC8.jpg)

マネージメント NSX エッジ サービス ゲートウェイと似ています。コンピュート NSX エッジ サービス ゲートウェイもまた、全てのインバウンド、アウトバンドのトラフィックを拒否するよう設定されています。VMware Cloud on AWS プラットフォームにプロビジョニングしたワークロード VM にアクセスするにはファイアウォール ルールを追加する必要があります

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC9.jpg)

Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access
Compute ゲートウェイにネイティブ AWS サービスからのインバウンド トラフィックのための

1\. *Network* タブの下の、*Compute Gateway* までナビゲートします

2\. *Firewall Rules* を拡げます

3\. *ADD RULE* をクリックします

<!--
#### AWS Inbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC10.jpg)

4\. Name - **AWS Inbound**

5\. Action - **Allow**

6\. Source - **All connected Amazon VPC**

7\. Destination - **192.168.#.0/24** (Where # is your student number)

8\. Service - **ANY**

9\. Click *SAVE* button.
-->

#### AWS インバウンド ファイアウォール ルールの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC10.jpg)

4\. **Name** - AWS Inbound

5\. **Action** - Allow

6\. **Source** - All connected Amazon VPC

7\. **Destination** - 192.168.#.0/24 (# は受講者番号)

8\. **Service** - ANY

9\. **SAVE** ボタンをクリックします。

<!--
#### Create AWS Outbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC11.jpg)

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1\. Name - **AWS Outbound**

2\. Action - **Allow**

3\. Source - **192.168.#.0/24** (Where # is your student number)

4\. Destination - **All connected Amazon VPC**

5\. Service - **ANY**

6\. Click *SAVE* button.

We have now successfully created rules which will allow us to access our vCenter server over port 443 from any location, in a real world deployment we would change this to only allow communication from a specific IP address range over a private link, once we have a VPN in place. We have also configured our Compute workloads to be able to communicate with any services in our native AWS VPC which our VMware Cloud on AWS environment is connected too.
-->

#### AWS アウトバウンド ファイアウォール ルールの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC11.jpg)

前のステップと同じ手順をなぞり、AWS アウトバウンド ファイアウォール ルールを以下の手順で作成します。

1\. Name - **AWS Outbound**

2\. Action - **Allow**

3\. Source - 192.168.#.0/24 (Where # is your student number)

4\. Destination - **All connected Amazon VPC**

5\. Service - **ANY**

6\. **SAVE** ボタンをクリックします。

これで、どこからでもポート 443 で我々の vCenter Server にアクセスを可能にするルールの作成に成功しました。実際の環境では、VPN などを張り、プライベートなリンク上の特定の IP アドレス レンジからの通信のみ許可するよう変更することになるでしょう。また、VMware Cloud on AWS に接続されたネイティブ AWS VPC で提供されるサービスに通信できるようにコンピュート ゲートウェイにも設定を行っています。

----------------

<!--
## Log into VMware Cloud on AWS vCenter

### Settings Tab

We will now login to our VMware Cloud on AWS vCenter instance, now that the required ports have been open on our Management NSX Edge Services Gateway.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC12.jpg)

1\. Login to your Student # SDDC and click on the *Settings* tab. This displays different
connection information for your VMware Cloud on AWS environment like:

2\. Default account information for your vCenter server

3\. URL to vCenter Server

4\. URL to vCenter API Explorer

5\. PowerCLI Connect string to be used if you desire to use PowerCLI to connect to your VMware
Cloud on AWS vCenter Server

6\. vCenter FQDN

7\. vCenter Public IP information

8\. Actual Public IP Address assigned to vCenter

9\. vCenter Private IP

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC13.jpg)

Click on the vSphere Client's HTML5 URL copy button (\#3 above), and login with
cloudadmin@vmc.local User Name and copy the password to your computer's clipboard and
paste it in the Password Field.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC14.jpg)

You have successfully logged in to the vCenter Server in your VMware Cloud on AWS environments as local user cloudadming@vmc.local.
-->

## VMware Cloud on AWS の vCenter へのログイン

### Settings タブ

マネージメント NSX エッジ サービス ゲートウェイで必要なポートがオープンされたので、これから VMware Cloud on AWS の vCenter インスタンスにログインします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-16-Image-16.png)

1\. 受講者番号の SDDC にログインし、**Settings** タブをクリックします。このタブは、受講者の VMware Cloud on AWS 環境への以下の接続情報を表示します

2\. あなたの vCenter Server のデフォルトのアカウント情報

3\. vCenter Server の URL

4\. vCenter Server API エクスプローラーの URL

5\. PowerCLI で VMware Cloud on AWS に接続する際に用いられる PowerCLI の接続文字列

6\. vCenter の FQDN

7\. DNS で解決される vCenter の IP アドレス

8\. 実際に vCenter に割り当てられている パブリック IP アドレス

9\. vCenter の プライベート IP アドレス

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC13.jpg)

vSphere HTML5 クライアントの URL コピー ボタン (\#3 にあります) をクリックし新しいブラウザのタブで開きます。cloudadmin@vmc.local ユーザー名をユーザー フィールドに入力し、パスワードのコピーボタンをクリックし、パスワード フィールドにペーストします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC14.jpg)

あなたの VMware Cloud on AWS 環境の vCenter Server へ、cloudadmin@vmc.local ローカル ユーザーとしてログインに成功しました。

<!--
## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment, or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

You can create two types of libraries: local or subscribed libraries.
-->

## コンテンツ ライブラリの作成

コンテンツ ライブラリは、仮想マシン テンプレート、vApp テンプレート および ISO ファイルなどのその他のタイプのファイルのためのコンテナ オブジェクトです。

vSphere Web クライアントでコンテンツ ライブラリを作成でき、コンテンツ ライブラリは仮想マシンや vApp を VMware Cloud on AWS の環境にデプロイするのに使うことができます。また、オンプレミスのデータセンターにすでにコンテンツ ライブラリを持っている場合は、そのコンテンツ ライブラリを使って SDDC にコンテンツをインポートすることができます。

ライブラリは、ローカル ライブラリおよび購読済みライブラリの 2 種類作成することができます。

<!--
### Local Libraries

You use a local library to store items in a single vCenter Server instance. You can publish the local library so that users from other vCenter Server systems can subscribe to it. When you publish a content library externally, you can configure a password for authentication.

VM templates and vApp templates are stored as an OVF file format in the content library. You can also upload other file types, such as ISO images, text files, and so on, in a content library.
-->

### ローカル ライブラリ

ローカル ライブラリは、一つの vCenter Server インスタンスにアイテムを保存するのに利用できます。他の vCenter Server システムのユーザーが購読 (Subscribe) するために、ローカル ライブラリを公開することができます。外部にコンテンツ ライブラリを公開する際は、認証のためのパスワードを設定することができます。

コンテンツ ライブラリに、VM テンプレートと vApp テンプレートは OVF ファイル フォーマットで保存で保存されます。また、ISO イメージ、テキスト ファイルなど その他のファイル タイプも、コンテンツ ライブラリにアップロードできます。

<!--
### Subscribed Libraries

You subscribe to a published library by creating a subscribed library. You can create the subscribed library in the same vCenter Server instance where the published library is, or in a different vCenter Server system. In the Create Library wizard you have the option to download all the contents of the published library immediately after the subscribed library is created, or to download only metadata for the items from the published library and later to download the full content of only the items you intend to use.

To ensure the contents of a subscribed library are up-to-date, the subscribed library automatically synchronizes to the source published library on regular intervals.

You can also manually synchronize subscribed libraries. You can use the option to download content from the source published library immediately or only when needed to manage your storage space.

Synchronization of a subscribed library that is set with the option to download all the contents of the published library immediately, synchronizes both the item metadata and the item contents. During the synchronisation the library items that are new for the subscribed library are fully downloaded to the storage location of the subscribed library.

Synchronization of a subscribed library that is set with the option to download contents only when needed synchronizes only the metadata for the library items from the published library, and does not download the contents of the items. This saves storage space. If you need to use a library item you need to synchronize that item. After you are done using the item, you can delete the item contents to free space on the storage. For subscribed libraries that are set with the option to download contents only when needed, synchronizing the subscribed library downloads only the metadata of all the items in the source published library, while synchronizing a library item downloads the full content of that item to your storage. If you use a subscribed library, you can only utilize the content, but cannot contribute with content. Only the administrator of the published library can manage the templates and files.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC15.jpg)

1\. In the vSphere HTML5 client, click on *Menu*

2\. Click on *Content Libraries*
-->

### 購読済みライブラリ

購読済みライブラリを作成することで、公開されたライブラリを購読 (Subscribe) することができます。購読済みライブラリは、公開されたライブラリがある同じ vCenter Server インスタンスにも作成でき、また異なる vCenter Server システムにも作成することができます。「ライブラリの作成」ウィザードでは、2 つの選択肢があります。購読済みライブラリが作成された後、公開されたライブラリの全てのコンテンツを直ぐにダウンロードするか、公開ライブラリのアイテムのメタデータのみをダウンロードし、アイテムを利用した際に後からそのアイテムの全てのコンテンツをダウンロードするかを選択できます。

購読済みライブラリのコンテンツが最新であることを保障するために、購読済みライブラリは、一定時間毎にソースとなる公開されたライブラリと自動的に同期します。

また、購読済みライブラリは手動でも同期できます。ソースの公開ライブラリから直ぐにコンテンツをダウンロードするオプションと、ストレージ容量を管理する場合には必要に応じてダウンロードするオプションを利用できます。

公開ライブラリのコンテンツを全て直ぐにダウンロードするオプションに設定された購読済みライブラリの同期は、アイテムのメタデータとアイテムのコンテンツの両方を同期させます。同期中、購読済みライブラリにとって新しいライブラリアイテムは、購読済みライブラリのストレージ ロケーションに完全にダウンロードされます。

必要に応じてコンテンツをダウンロードするオプションに設定された購読済みライブラリの同期は、公開ライブラリからライブラリ アイテムのメタデータのみを同期し、アイテムのコンテンツはダウンロードしません。これはストレージ容量を節約できます。ライブラリ アイテムを利用する必要がある際は、そのアイテムを同期させる必要があります。そのアイテムを利用した後は、ストレージのスペースを開放するためにそのアイテムのコンテンツを削除できます。<!-- 次の文は繰返しとなるのと、While 以降の意味が通じない --> 購読済みライブラリを用いる場合、コンテンツを利用するのみで、コンテントを追加することはできません。公開ライブラリの管理者のみが、テンプレートやファイルを管理できます。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-19-Image-19.png)

1\. vSphere HTML5 クライアントにて、**Menu** をクリックします

2\. **Content Libraries** をクリックします

<!--
### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC16.jpg)

1\. In your Content Library window, click the *+* sign to add a new Content Library.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC17.jpg)

2\. Name your Content Library *Student#* (where # is the number assigned to you)

3\. (Optional) Enter some notes for your Content Library

4\. Click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC18.jpg)

5\. Select *Subscribed content library*

6\. Under *Subscription URL* enter the following:

```
https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/4aa185b4-3d6e-45b4-90ca-cd3a845d4502/lib.json
```

7\. Click the checkbox for *Enable Authentication*

8\. For the *Password* enter: **VMware1!**

9\. Ensure Download content is set to *Immediately*

10\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC19.jpg)

11\. Highlight the *WorkloadDatastore* as the storage location

12\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC20.jpg)

13\. Click *Finish*. Your content library should take about ~20 minutes to complete syncing.

You have now successfully subscribed to a vSphere content library from your VMware Cloud on AWS vCenter instance.
-->

### 既存のコンテンツ ライブラリの購読

オンプレミスのデータセンターに既にコンテンツ ライブラリがあれば、SDDC にコンテンツをインポートするためにそのコンテンツ ライブラリを利用できます。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC16.jpg)

1\. コンテンツ ライブラリ ウィンドウにて、**+** マークをクリックし新しいコンテンツ ライブラリを追加します。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC17.jpg)

2\. コンテンツ ライブラリの名前を **Student#** とします。(# は受講者番号)

3\. (オプション) コンテンツ ライブラリのノートを適当に入力します。

4\. **Next** ボタンをクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC18.jpg)

5\. **Subscribed content library** を選択します。

6\. Under **Subscription URL** の下に以下を入力します。

```
https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/4aa185b4-3d6e-45b4-90ca-cd3a845d4502/lib.json
```

7\. **Enable Authentication** チェック ボックスをクリックします。

8\. **Password** は **VMware1!** と入力します。

9\. ダウンロード コンテントが **Immediately** に設定されていることを確認します。

10\. **Next** をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC19.jpg)

11\. ストレージ ロケーションとして **WorkloadDatastore** をハイライトします。

12\. **Next** をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC20.jpg)

13\. **Finish** をクリックします。コンテンツ ライブラリが同期を完了するまで 20 分ほど掛かります。

これで、あなたの VMware Cloud on AWS の vCenter インスタンスから、vSphere のコンテンツ ライブラリを購読することに成功しました

<!--
### Create a Local Content Library

We will now create a local content library for this VMware Cloud on AWS vCenter instance.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC21.jpg)

1\. Click the *+* sign to create a new Content Library

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC22.jpg)

2\. Name your new Content Library: **LocalContentLibrary#** (where # is your student #)

3\. (Optional) Enter some notes about your Content Library

4\. Click *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC23.jpg)

5\. Make sure *Local content library* is selected

6\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC24.jpg)

7\. Highlight the *WorkloadDatastore* as the storage location

8\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC25.jpg)

9\. Review your information and click *Finish*

Congratulations, you have created your Local Content Library.
-->

### ローカル コンテンツ ライブラリの作成

これから、VMware Cloud on AWS の vCenter インスタンスにローカル コンテンツ ライブラリを作成します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC21.jpg)

1\. **+** マークをクリックし、新しいコンテンツ ライブラリを追加します。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC22.jpg)

2\. コンテンツ ライブラリの名前を **LocalContentLibrary#** とします。(# は受講者番号になります。)

3\. (オプション) コンテンツ ライブラリのノートを適当に入力します。

4\. **Next** ボタンをクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC23.jpg)

5\. **Local content library** が選択されていることを確認します。

6\. **Next** をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC24.jpg)

7\. ストレージ ロケーションとして **WorkloadDatastore** をハイライトさせます。

8\. **Next** をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC25.jpg)

9\. 入力した情報を確認し、**Finish** をクリックします。

おめでとうございます。ローカル コンテンツ ライブラリが作成されました。

<!--
## Create a Logical Network

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC26.jpg)

1\. Once you are logged in to your vCenter Server Click on *Menu*

2\. Select *Global Inventory Lists*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC27.jpg)

3\. Click on *Logical Networks* in the left pane

4\. Click on the *Add* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC28.jpg)

5\. Name your New Logical Network *Student#-LN* (where # is your student number)

6\. Select the *Routed Network* radio button

7\. For CIDR Block enter **192.168.#.0/24** (where # is your student #)

    • If your designated student number is between 1 and 9, your CIDR block should look like this: **192.168.1.0/24** - This example represents student number 1

    • For students 10 thru 20 it should look like this: **192.168.10.0/24** - This example represents student number 10

8\. Enter **192.168.#.1** for the Default Gateway IP - Example: 192.168.1.1

9\. Make sure DHCP is Enabled by clicking on the *checkbox*

10\. Enter **192.168.#.100-192.168.#.200** for IP Range

11\. Type **corp.local** as your DNS Domain Name

12\. Click *OK* to create your new logical network
-->

## 論理ネットワークの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC26.jpg)

1\. vCenter Server にログインし、**メニュー** をクリックします。

2\. **グローバル インベントリ リスト** を選択します。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC27.jpg)

3\. 左ペインから **論理ネットワーク** をクリックします。

4\. **追加** ボタンをクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC28.jpg)

5\. 新しい論理ネットワークの名前を **Student#-LN** とします。(# は受講者番号)

6\. **Routed Network** が選択されていることを確認します。

7\. CIDR ブロックに **192.168.###.0/24** を入力します。(# は受講者番号)

    • 受講者番号が 1 から 9 の場合、CIDR ブロックは次のようになります。**192.168.1.0/24** - この例は受講者番号 1 の場合です
    • 受講者番号が 10 から 20 の場合、CIDR ブロックは次のようになります。**192.168.10.0/24** - この例は受講者番号 10 の場合です

8\. デフォルト ゲートウェイ IP に **192.168.###.1** を入力します。- 例: 192.168.1.1

9\. **チェック ボックス** をクリックして、DHCP が 有効であることを確認します。

10\. IP レンジに **192.168.###.100-192.168.###.200** を入力します。

11\. DNS ドメイン名として **corp.local** を入力します。

12\. **OK** をクリックし、新しい論理ネットワークを作成します。

<!--
## Create Linux Customization Spec

When you clone a virtual machine or deploy a virtual machine from a template, you can customize the guest operating system of the virtual machine to change properties such as the computer name, network settings, and license settings.

Customizing guest operating systems can help prevent conflicts that can result if virtual machines with identical settings are deployed, such as conflicts due to duplicate computer names.

You can specify the customization settings by launching the Guest Customization wizard during the cloning or deployment process. Alternatively, you can create customization specifications, which are customization settings stored in the vCenter Server database. During the cloning or deployment process, you can select a customization specification to apply to the new virtual machine.

Use the Customization Specification Manager to manage customization specifications you create with the Guest Customization wizard.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC29.jpg)

1\. In the vCenter HTML5 client click *Menu*

2\. Click *Policies and Profiles*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC30.jpg)

3\. Click on *+ New* to add a new Customization Specification

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC31.jpg)

4\. Give your VM Customization Spec a name, such as *Linux Spec*

5\. Enter a description for it (Optional)

6\. Make sure to select *Linux* in the Guest OS, Target guest OS section

7\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC32.jpg)

8\. Click on the third option, *Enter a name* button

9\. Enter a name for your linux VMs, such as *linux-vm*

10\. Click on the *Append a numeric value* checkbox

11\. Enter **corp.local** for the Domain Name

12\. Click the *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC33.jpg)

13\. Select *US* for Area

14\. Select *Eastern* for Location

15\. Select *Local time*

16\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC34.jpg)

17\. Leave the defaults on the *Network* screen and click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC35.jpg)

18\. Under Primary DNS Server enter **10.46.159.10** and leave the Secondary DNS Server and Tertiary DNS Server blank

19\. Type **corp.local** for DNS Search Paths and click **Add**

20\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC36.jpg)

21\. Review your entries and click *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC37.jpg)

Congratulations! You have successfully created your VM Customization Spec for your Linux VMs. You can also export (Duplicate), Edit, Import, and Export a VM Customization Spec.
-->

## Linux 向けカスタマイズ仕様の作成

仮想マシンをクローンする際、あるいは、テンプレートから仮想マシンをデプロイする際、コンピューター名、ネットワーク設定やライセンス設定を変更することで仮想マシンのゲスト オペレーティング システムをカスタマイズすることができます。

ゲスト オペレーティング システムをカスタマイズすることは、重複したコンピューター名により名前が衝突するといった、同一の設定を持つ仮想マシンがデプロイされることによる衝突を防ぐ助けとなります。

デプロイやクローンのの途中でゲスト カスタマイゼーション ウィザードを起動することによって、カスタマイズ設定を行うことができます。あるいは、vCenter Server のデータベースにカスタマイズ設定が保存されるカスタマイズ仕様を作成することもできます。クローンやデプロイの途中で、カスタマイズ仕様を選択し新しい仮想マシンに適用することができます。

ゲスト カスタマイゼーション ウィザードで作成したカスタマイズ仕様を管理するために、カスタマイズ仕様マネージャを利用して下さい。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC29.jpg)

1\. vSphere HTML5 クライアントにて **Menu** をクリックします
2\. **Policies and Profiles** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC30.jpg)

3\. Click on **+ New** をクリックし、新しい Linu 向けカスタマイズ仕様を追加します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC31.jpg)

4\. カスタマイズ仕様に *Linux Spec* などといった名前を付けます

5\. カスタマイズ仕様に説明を付け加えます。(オプショナル)

6\. **Linux** を選択します

7\. **Next** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC32.jpg)

8\. **Enter a name** ボタンをクリックします。

9\. ターゲット ゲスト OS セクションにて、Linux 仮想マシンの名前を入力します

10\. **Append numeric value** チェック ボックスをクリックします

11\. ドメイン名に **corp.local** を入力します

12\. **Next** ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC33.jpg)

13\. エリアで **US** を選択します

14\. 保存場所で **Eastern** を選択します

15\. **Local time** を選択します

16\. **Next** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC34.jpg)

17\. **Network** スクリーンはデフォルトのままとし、**Next** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC35.jpg)

18\. プライマリ DNS サーバーに **10.46.159.10** と入力します

19\. DNS 検索パスに **corp.local** と入力します

20\. **Next** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC36.jpg)

21\. 入力した項目を確認し、**Finish** をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC37.jpg)

おめでとうございます。Linux 仮想マシンのためのカスタマイズ仕様が作成されました。カスタマイズ仕様は、複製、編集、インポート、エクスポートすることができます

<!--
## Deploy Virtual Machines

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC38.jpg)

1\. On your Content Libraries *(Menu -> Content Libraries)*, select *Student#* and select the *Templates* tab.

2\. Right click on the *centos01-web* template and select *New VM from This Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC39.jpg)

3\. Name your Virtual Machine *StudentVM#* (where # is your student number)

4\. Expand the location area until you see *Workloads* and highlight it

5\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC40.jpg)

6\. Expand the destination compute resources until you find *compute-ResourcePool*, select it

7\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC41.jpg)

8\. Click the *Next* button on the Review details screen

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC42.jpg)

9\. In the *Select storage* step, highlight *WorkloadDatastore*

10\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC43.jpg)

11\. In the *Select networks* step, click the drop down box to select the Destination Network (you may need to click Browse to see other networks and select your *Student#-LN* network you created previously

12\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC44.jpg)

13\. In the *Ready to complete* section, review and ensure all your selections are correct and click *Finish*
-->

## 仮想マシンのデプロイ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC38.jpg)

1\. コンテンツ ライブラリ (メニュー -> コンテンツ ライブラリ) にて、**Student#** を選択し、**Templates** タブを選択します。

2\. **centos01-web** テンプレートを右クリックし、**New VM from This Template** を選択します。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC39.jpg)

3\. 仮想マシンの名前を **StudentVM#** とします。(# は受講者番号となります。)

4\. **Workloads** が見付かるまでロケーション エリアを展開し、これをハイライトします。

5\. **Next** をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC40.jpg)

6\. **Compute-ResourcePool** が見付かるまで Destination コンピュート リソースを展開し、これを選択します。

7\. **Next** ボタンをクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC41.jpg)

8\. 設定の確認スクリーンで **Next** ボタンをクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC42.jpg)

9\. **Select storage** ステップにて、**WorkloadDatastore** をハイライトします。

10\. **Next** をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC43.jpg)

11\. **Select networks** ステップで、ドロップ ダウン リストをクリックして Destination Network を選択します。(他のネットワークを参照したり、事前に作成した "Student#-LNStudent#-LN" ネットワークを選択するために、Browse をクリックする必要があるかも知れません)

12\. **Next** をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC44.jpg)

13\. **Ready to complete** セクションにて、全てのセクションが正しいことを確認し、**Finish** をクリックします。

<!--
## Convert Virtual Machine to Template

In this step you will be cloning your newly created Virtual Machine into a Template for later use in vRealize Automation section.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-38-Image-49.png)

1. Ensure your VM deployment completed from your previous step
2. Click on **Menu**
3. Select **VMs and Templates**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-50.png)
4. Select your newly created VM **Student#** (where # is your student number)
5. Click on **Template**
6. Select **Convert to Template**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-51.png)
7. Click **Yes**  in the Convert to Template prompt

You have completed this step.
-->

## 仮想マシンをテンプレートに変換

このステップでは、後続の vRealize Automation セクションのために、新しく作成された仮想マシンを仮想マシンにクローンします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-38-Image-49.png)

1\. 前のステップの仮想マシンのデプロイが完了していることを確認します

2\. **Menu** をクリックします

3\. **VMs and Templates** を選択します

    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-50.png)

4\. 新しく作成された仮想マシン **Student#** を選択します。(# は受講者番号になります)

5\. **Template** をクリックします

6\. **Convert to Template** を選択します

    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-51.png)

7\. 変換の確認 ダイアログで **Yes** をクリックします

このステップを完了しました。