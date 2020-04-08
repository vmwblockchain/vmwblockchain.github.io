---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2019-03-19
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---

<!--
This documentation is translated into Japanese from following commit.
commit 7755536cd867d7448f34a100a94cecf3ddd2f997
-->

<!--
# Introduction

In this lab we are going to start with looking at the basic tasks which you will perform in the VMware Cloud on AWS user interface when you are administering the platform.
-->

# はじめに

このラボでは、プラットフォームを管理する際に VMware Cloud on AWS のユーザーインターフェースで行う基本的なタスクを見ていくことから始めます。

<!--
## Viewing your SDDC

![SDDC-Network-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc01.jpg)

After you login, you should see a single SDDC in the user interface following the naming format Student-Workshop-#.#. An SDDC is a fully deployed environment including vSphere, NSX, vSAN and vCenter Server. Deployment of a fully configured SDDC takes about two hours so for the purposes of this lab, we have already deployed it for you. This SDDC is in the same state it would be if you have deployed it. Let's take a look at the SDDC properties.

1. First click on View Details to open the SDDC properties.

![SDDC-Network-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc02.jpg)

You will start with the Summary of the SDDC. There are a number of other tabs available as follows:
1. Support - You can contact Support with your SDDC ID, Org ID, vCenter Private and Public IPs and the date of your SDDC Deployment.
2. Settings: Gives you access to your vSphere Client (HTML5), vCenter Server API, PowerCLI Connect, vCenter Server and reviews your Authentication information.
3. Troubleshooting: Allows you to run network connectivity tests to ensure all necessary access is available to perform select use cases.
4. Add Ons: Here you will find Add On services for your VMware Cloud on AWS environment like Hybrid Cloud Extension and VMware Site Recovery.
5. Networking & Security: Provides a full diagram of the Management and Compute Gateways.  This is where you can configuration locgical networks, VPN's and firewall rules. We will cover this in more detail later. Click on Networking & Security to proceed to the next article to learn more about VMware Cloud on AWS Network and Security Configuration.
-->

## SDDC の表示

![SDDC-Network-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc01.jpg)

ログインすると、Student-Workshop-#.# というフォーマットの名前のシングル SDDC があります。SDDC は vSphere、NSX、vSAN そして vCenter Server を含む環境のデプロイが完了しています。このラボのために完全に構成された SDDC のデプロイはおよそ 2 時間ほど掛かります。今回のワークショップでは、これは既にデプロイ済みとなります。この SDDC は、皆様がデプロイした時と同じ状態になっています。それでは SDDC のプロパティを見ていきましょう。

1. SDDC プロパティを開くために「詳細を表示」を最初にクリックしてください

![SDDC-Network-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc02.jpg)

まず SDDC のサマリが表示されます。さらに次にあげるいくつかのタブが用意されています:
1. サポート - SDDC ID、組織 ID、vCenter のプライベート IP、パブリック IP、および SDDC がデプロイされた日付を確認できます。サポートに連絡する際にこれらの情報を使うことができます。
2. 設定: vSphere Client（HTML5）、vCenter Server API、PowerCLI 接続、vCenter Server の FQDN および認証情報を確認できます。
3. トラブルシューティング: 選択したユースケースのために必要なアクセスが確保できているかを確認するために、接続性確認ツールを実行できます。
4. アドオン: Hybrid Cloud Extension や VMware Site Recovery といった VMware Cloud on AWS のためにアドオンサービスがあります。
5. ネットワークとセキュリティ: 管理ゲートウェイとコンピュートゲートウェイの完全なダイアグラムを確認できます。ここでは、論理ネットワーク、VPN、ファイアウォールの構成を行えます。これらについては後述でより詳細な内容を見ていきます。次の項目に進むために「ネットワークとセキュリティ」をクリックしてください。VMware Cloud on AWS のネットワークとセキュリティの構成について見ていきます。

<!--
## Create a Logical Network

![SDDC-Network-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc03.jpg)

From the previous article, you should see the Network & Security information for the SDDC.
VMware Cloud on AWS allows you to quickly and easily create new logical network segments on
demand. Let's create a new network segment in the SDDC.

1. Click the **Networking & Security** tab, then click on **Segments** to show all of the existing network segments.
2. Click on **Add Segments** to create a new network segment.
3. Enter **Demo-Net** for the Name of the new network segment.
4. For the Gateway/Prefifix Length enter 10.10.xx.1/24 (xx depicts your student number). This represents the default gateway.
of the network and the prefix length of the network. For more details on IP addressing see below.
5. For **DHCP**, click the down arrow and select Enabled to enable DHCP on the network.
6. Enter **10.10.xx.10-10.10.xx.200** for the **DHCP IP Range**. This is the range of IP addresses the DHCP server will grant to workloads attached to the network.
7. Click **Save** to save the logical network.
-->

## セグメント（論理ネットワーク）の作成

![SDDC-Network-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc03.jpg)

前の項目で、SDDC のネットワークとセキュリティの情報を確認しました。
VMware Cloud on AWS では、必要に応じて簡単且つ迅速に新しいセグメントを作成することができます。それでは SDDC に新しいネットワーク セグメントを作成しましょう。

1. **ネットワークとセキュリティ** タブをクリックし、次に既存のネットワーク セグメントを全て表示するために **セグメント** をクリックします。
2. 新しいネットワーク セグメントを追加するために **セグメントの追加** をクリックします。
3. 新しいネットワーク セグメントの名前 **Demo-Net** を入力します。
4. ゲートウェイとプリフィックス長のために、10.10.xx.1/24（xx は受講者番号になります）を入力します。これはデフォルト ゲートウェイとなります。
5. **DHCP** については、ネットワーク上で DHCP を有効にするために下矢印をクリックして「有効」を選択します。
6. **DHCP IP アドレス範囲** に **10.10.xx.10-10.10.xx.200** と入力します。これは、ネットワークに接続されるワークロードに DHCP サーバーが付与する IP アドレスの範囲になります。
7. セグメント（論理ネットワーク）を保存するために、**保存** をクリックします。

<!--
**Note: Make sure you leave the default of Routed for Type and do not enter anything for the DNS suffix.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> CIDR notation is a compact representation of an IP address and its associated routing prefix. The notation is constructed from an IP address, a slash('/') character, and a decimal number. The number is the count of leading bits in the routing mask, traditionally called the network mask.  The IP address is expressed according to the standards of IPv4 or IPv6.
<br/>
<br/>
The address may denote a single, distinct interface address or the beginning address of an entire network. The maximum size of the network is given by the number of addresses that are possible with the remaining, least-significant bits below the prefix.  The aggregation of these bits is often called the host identifier.
<br/>
<br/>
For example:
<br/>
<br/>
• 192.168.100.14/24 represents the IPV4 address 192.168.100.14 and its associated routing prefix 192.168.100.0, or equivalently, its subnet mask 255.255.255.0, which has 24 leading 1-bits.
<br/>
<br/>
• The IPV4 block 192.168.100.0/22 represents the 1024 IPV4 addresses from 192.168.100.0 to 192.168.103.255.
</font>
</aside>
-->

**注意: タイプはデフォルトのルーティングのままとなっていることを確認してください。また DNS サフィックスは何も入力しないで下さい。**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> CIDR の表記法は、IP アドレスとそれに関連したルーティング プリフィックスのコンパクトな表記です。この表記法は、IP アドレス、スラッシュ文字（'/'）、そして十進数から構成されます。受信数の番号は、ルーティング マスクのビット数となります。従来からネットワーク マスクとも呼ばれます。IP アドレスは IPv4 または IPv6 の標準的な表記となります。
<br/>
<br/>
このアドレスは、確たるインターフェースのアドレスかネットワークの最初のアドレスが一つ使われます。ネットワークの最大サイズは、プリフィックス以下のビットで表現可能なアドレスの数となります。このビットの集合はホスト ID とも呼ばれます。
<br/>
<br/>
具体例:
<br/>
<br/>
• 192.168.100.14/24 は IPv4 のアドレス 192.168.110.14 と、それに関連したルーティング プリフィックス 192.168.100.0 を表します。あるいは、24 個のビットをもつサブネット マスク 255.255.255.0 と等しくなります。
<br/>
<br/>
• IPv4 ブロック 192.168.110.0/22 は、192.168.100.0 から 192.168.100.255 までの 1024 個の IP アドレスを表します。
</font>
</aside>

<!--
## Verify Network Segment Configuration

![SDDC-Network-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc04.jpg)

1. Verify the network segment was added correctly.  Your information should match the highlighted area above.


## Configure Firewall Rule for vCenter Access

![SDDC-Network-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc05.jpg)

By default, all inbound firewall rules are set to Deny in VMware Cloud on AWS. In order to access vCenter server, we will need to configure a firewall rule allowing inbound access.

**Note: In most enterprise environments, you would create VPN or Direct Connect VIF to allow limited access firewall rules to vCenter. In this environment, we will open it to any IP address on the internet which is not recommended.**

1. Click on **Gateway Firewall** on the lefthand side of the screen.
2. If it is not already selected, click on **Management Gateway** to create a firewall rules that allow access to management components in the SDDC.
3. Click **Add New Rule** to add a new rule to the edge gateway.
4. For the **Name** enter **vCenter Inbound Rule**.
5. Click **Set Source** to define the source for the firewall rule.
-->

## ネットワークセグメントの構成確認

![SDDC-Network-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc04.jpg)

1. ネットワーク セグメントの構成が正しく追加されたことを確認します。構成は上図のハイライトされた部分と一致しているはずです。 


## vCenter へのアクセスのためのファイアウォール ルールの構成

![SDDC-Network-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc05.jpg)

デフォルトでは、VMware Cloud on AWS の全てのインバウンド ファイアウォール ルールは拒否に設定されています。vCenter Server にアクセスするために、インバウンドアクセスを許可するファイアウォール ルールを構成する必要があります。

**殆どのエンタープライズ向けの環境では、vCenter への制限されたアクセスのためのファイアウォール ルールを VPN か Direct Connect に適用すると思われます。この環境ではインターネットに全ての IP アドレスに対してオープンに設定しますが、これは推奨されません。**

1. スクリーン左側にて、**ゲートウェイ ファイアウォール** をクリックします。
2. もし選択されていないのであれば、SDDC で管理コンポーネントへのアクセスを許可するファイアウォール ルールを作成するために **管理ゲートウェイ** をクリックします。
4. 管理ゲートウェイに新しいルールを追加するために **新しいルールの追加** をクリックします。
5. **名前** に **vCenter Inbound Rule** と入力します。
6. ファイアウォール ルールの送信元を定義するために **送信元の設定** をクリックします。

<!--
### Select the Firewall Rule Source

![SDDC-Network-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc06.jpg)

1. Click the **Radio Button next** to **Any**.
2. Click **Save** to save the source information in the rule.
-->

### ファイアウォール ルールの送信元の選択

![SDDC-Network-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc06.jpg)

1. **Any** のとなりにある **ラジオボタン** をクリックします。
2. このルールの送信元の情報を保存するために **SAVE** をクリックします。

<!--
### Configure Firewall Rule for vCenter Access (Continued)

![SDDC-Network-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc07.jpg)

1. Click **Set Destination** to launch a new window to set the destination for the rule.
-->

### vCenter へのアクセスのためのファイアウォール ルールの構成（続き）


![SDDC-Network-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc07.jpg)

1. ファイアウォール ルールの宛先を設定するウィンドウを開くために **宛先の設定** をクリックします。

<!--
### Select the Firewall Rule Destination

![SDDC-Network-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc08.jpg)

1. Click the **Radio Button** next to **System Defined Groups**.
2. Select the **Checkbox** next to **vCenter**.
3. Click **Save** to save the destination information in the rule.
-->

### ファイアウォール ルールの宛先の選択

![SDDC-Network-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc08.jpg)

1. **System Defined Group** のとなりにある **ラジオボタン** をクリックします。
2. **vCenter** のとなりの **ラジオボタン** を選択します。
3. このルールの宛先情報を保存するために **SAVE** をクリックします。

<!--
### Configure Firewall rule for vCenter Access (Contined)

![SDDC-Network-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc09.jpg)

Continue configuring the vCenter Inbound Rule:

1. Click box below **Services** and select **HTTPS (TCP 443)** to allow SSL access to the vCenter server.
2. Publish the rules by clicking **Publish** button to activate the firewall rule.

vCenter should now be accessible from anywhere in the internet.  in the next section, we will access vCenter HTML5 client to being configuring virtual machines.
-->

### vCenter へのアクセスのためのファイアウォール ルールの構成（続き）

![SDDC-Network-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc09.jpg)

vCenter Inbound Rule の構成を続けます:

1. vCenter Server への SSL アクセスを許可するために、**サービス** の列にあるボックスをクリックし、**HTTPS (TCP 443)** を選択染ます。
2. ファイアウォール ルールを有効化するために **発行** ボタンをクリックします。

インターネットのどこからでも vCenter がアクセスできるようになりました。次の項目では、仮想マシンを構成していくために vCenter HTML5 クライアントにアクセスします。

<!--
## Log into VMware Cloud on AWS vCenter

![SDDC-vcenter-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc010.jpg)

The settings to connect to the vCenter server associated with the SDDC is available on the setting tab for the SDDC. Let's connect to the vCenter server and login.

1. Click on the **Settings** tab for the SDDC we configured in the last lesson.
2. Click the **arrow** next to Default vCenter User Account to expose the login details. In this lab we will use the default cloudadmin@vmc.local user.
3. Copy the **password** by clicking the two squares next to the password. This will copy it to the consoles clipboard.
4. Click the **arrow** next to **vSphere Client (HTML5)** to expose the URL for vCenter.
5. Click the **URL** link to **open** the vSphere Client in another tab.

**NOTE: If you experience any login issues below, you can click the two boxes next to the URL below to paste the URL into an incognito window. This should not be needed normally.**
-->

## VMware Cloud on AWS の vCenter へログイン

![SDDC-vcenter-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc010.jpg)

SDDC の vCenter Server に接続するための設定は、SDDC の設定タブにあります。それでは vCenter Server に接続しログインしましょう。

1. これまで構成してきた SDDC の **設定** タブをクリックします。
2. ログイン情報を表示するために「デフォルトの vCenter Server ユーザー アカウント」のとなりにある **矢印** をクリックします。このラボではデフォルトの cloudadmin@vmc.local を利用します。
3. パスワードのとなりにある 2 つに重なった四角をクリックし、**パスワード** をコピーします。
4. vCenter Server の URL を表示するために、**vSphere Client (HTML5)** のとなりにある **矢印** をクリックします。
5. 別のタブで vSphere Client を開くために **URL** リンクをクリックします。

**注意: もし以下の手順でなんらかのログインの問題がある場合には、URL のとなりにある 2 つに重なった四角をクリックし URL をコピーし、シークレット ウィンドウに URL をペーストします。これは通常必要ないはずです。**

<!--
### Login to the vSphere Web Client

To login to the vSphere Web Client:

![SDDC-vcenter-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc011.jpg)

1. In the User name field enter **cloudadmin@vmc.local.**
2. Right-click in the **Password** field and paste the password copied in the previous step.
3. Click **Login**.
-->

### vSphere Client (HTML5) へのログイン

vSphere Client (HTML5) へログインするには:

![SDDC-vcenter-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc011.jpg)

1. ユーザー名フィールドに **cloudadmin@vmc.local.** を入力します。
2. 前のステップでコピーしたパスワードを、**パスワード** フィールドを右クリックしてペーストします。
3. **ログイン** をクリックします。

<!--
### vSphere Web Client

![SDDC012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc012.jpg)

You are now logged in to your VMware Cloud AWS vCenter Server as cloudadmin@vmc.local user.
-->

### vSphere Client (HTML5)

![SDDC012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc012.jpg)

これで cloudadmin@vmc.local ユーザーとして VMware Cloud on AWS の vCenter Server にログインできました。

<!--
## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

You can create two types of libraries: local or subscribed library.

**Local Libraries**

You use a local library to store items in a single vCenter Server instance. You can publish the local library so that users from other vCenter Server systems can subscribe to it. When you publish a content library externally, you can configure a password for authentication.

VM templates and vApps templates are stored as OVF file formats in the content library. You can also upload other file types, such as ISO images, text files, and so on, in a content library.
-->

## コンテンツ ライブラリの作成

コンテンツ ライブラリは、仮想マシン テンプレート、vApp テンプレート および ISO ファイルなどのその他のタイプのファイルのためのコンテナ オブジェクトです。

vSphere Client でコンテンツ ライブラリを作成できます。コンテンツ ライブラリは、仮想マシンや vApp を VMware Cloud on AWS の環境にデプロイするために使うことができます。また、オンプレミスのデータセンターにすでにコンテンツ ライブラリを持っている場合は、そのコンテンツ ライブラリを使って SDDC にコンテンツをインポートすることもできます。

ライブラリは、ローカル ライブラリおよび購読済みライブラリの 2 種類作成することができます。

**ローカル ライブラリ**

ローカル ライブラリは、一つの vCenter Server インスタンスにアイテムを保存するのに利用できます。他の vCenter Server システムのユーザーが購読 (Subscribe) するために、ローカル ライブラリを公開することができます。外部にコンテンツ ライブラリを公開する際は、認証のためのパスワードを設定することができます。

VM テンプレートと vApp テンプレートは、コンテンツ ライブラリに OVF ファイル フォーマットで保存で保存されます。また、ISO イメージ、テキスト ファイルなど その他のファイル タイプも、コンテンツ ライブラリにアップロードできます。

<!--
**Subscribed Libraries**

You subscribe to a published library by creating a subscribed library. You can create the subscribed library in the same vCenter Server instance where the published library is, or in a different vCenter Server system. In the Create Library wizard you have the option to download all the contents of the published library immediately after the subscribed library is created, or to download only metadata for the items from the published library and later to download the full content of only the items you intend to use.

To ensure the contents of a subscribed library are up-to-date, the subscribed library automatically synchronizes to the source published library on regular intervals. You can also manually synchronize subscribed libraries.

You can use the option to download content from the source published library immediately or only when needed to manage your storage space.

Synchronization of a subscribed library that is set with the option to download all the contents of the published library immediately, synchronizes both the item metadata and the item contents. During the synchronisation the library items that are new for the subscribed library are fully downloaded to the storage location of the subscribed library.

Synchronization of a subscribed library that is set with the option to download contents only when needed synchronizes only the metadata for the library items from the published library, and does not download the contents of the items. This saves storage space. If you need to use a library item you need to synchronize that item. After you are done using the item, you can delete the item contents to free space on the storage. For subscribed libraries that are set with the option to download contents only when needed, synchronizing the subscribed library downloads only the metadata of all the items in the source published library, while synchronizing a library item downloads the full content of that item to your storage.

If you use a subscribed library, you can only utilize the content, but cannot contribute with content. Only the administrator of the published library can manage the templates and files.
-->

**サブスクライブ済みライブラリ**

サブスクライブ済みライブラリを作成することで、公開されたライブラリをサブスクライブすることができます。サブスクライブ済みライブラリは、公開されたライブラリがある同じ vCenter Server インスタンスにも作成でき、また異なる vCenter Server システムにも作成することができます。「ライブラリの作成」ウィザードでは、2 つの選択肢があります。サブスクライブ済みライブラリが作成された後、公開されたライブラリの全てのコンテンツを直ぐにダウンロードするか、公開ライブラリのアイテムのメタデータのみをダウンロードしアイテムを利用する際に後からそのアイテムの全てのコンテンツをダウンロードするかを選択できます。

サブスクライブ済みライブラリのコンテンツが最新であることを保証するために、サブスクライブ済みライブラリは、一定時間毎にソースとなる公開されたライブラリと自動的に同期します。また、手動でサブスクライブ済みライブラリを同期させることもできます。

ソースの公開ライブラリから直ぐにコンテンツをダウンロードするオプションと、ストレージ容量を節約するために必要に応じてダウンロードするオプションのどちらかを利用できます。

公開ライブラリのコンテンツを全て直ぐにダウンロードするオプションに設定されたサブスクライブ済みライブラリの同期は、アイテムのメタデータとアイテムのコンテンツの両方を同期させます。同期中、サブスクライブ済みライブラリにとって新しいライブラリアイテムは、サブスクライブ済みライブラリのストレージ ロケーションに全てダウンロードされます。

必要に応じてコンテンツをダウンロードするオプションに設定されたサブスクライブ済みライブラリの同期は、公開ライブラリからライブラリ アイテムのメタデータのみを同期し、アイテムのコンテンツはダウンロードしません。これによりストレージ容量を節約できます。ライブラリ アイテムを利用する必要がある際は、そのアイテムを同期させる必要があります。そのアイテムを利用した後は、ストレージのスペースを開放するためにそのアイテムのコンテンツを削除できます。必要に応じてコンテンツをダウンロードするオプションに設定されたサブスクライブ済みライブラリでは、ライブラリのアイテムの同期ではストレージに全てのコンテンツをダウンロードするのに対し、サブスクライブ済みライブラリの同期はソースとなる公開ライブラリの全てのアイテムのメタデータのみをダウンロードします。

サブスクライブ済みライブラリを使う場合、コンテンツを追加することはできず、コンテンツは利用することしかできません。公開ライブラリの管理者のみが、テンプレートやファイルを管理することができます。

<!--
### Access Content Libraries in the vSphere Client

![SDDC013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

1. Click on **Menu**
2. Click on **Content Libraries**
-->

### vSphere Client のコンテンツ ライブラリへアクセス

![SDDC013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

1. **メニュー** をクリックします。
2. **コンテンツ ライブラリ** をクリックします。

<!--
### Subscribe to an existing Content Library

![SDDC014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc014.jpg)

1. In your Content Library window, click the **+ (plus)** sign to add a new Content Library.

![SDDC015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc015.jpg)

1. Enter **VMC Content Library** for the Name of the library.
2. Click the **Next** button.

![SDDC016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc016.jpg)

1. Select the radio button next to **Subscribed content library.**
2. Under **Subscription URL** enter the following: https://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json
3. Leave the checkbox **unchecked** next to **Enabled Authentication**.
4. Make sure Download content is set to **immediately**.
5. Click **Next** to continue.

![SDDC017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc017.jpg)

1. Click on **WorkloadDatastore** for content library storage.
2. Click the **Next** button.

![SDDC018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc018.jpg)

1. Click the **Finish** button.

**Note:  Depending the size and number of templates it can take a while to sync the content.  This content library should only take a few minutes to synchronize.**
-->

### 既存のコンテンツ ライブラリのサブスクライブ

![SDDC014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc014.jpg)

1. 新しいコンテンツ ライブラリを追加するために、コンテンツ ライブラリ ウィンドウにて **+（プラス）** マークをクリックします。

![SDDC015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc015.jpg)

1. ライブラリの名前に **VMC Content Library** と入力します。
2. **Next** ボタンをクリックします。

![SDDC016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc016.jpg)

1. **サブスクライブ済みコンテンツ ライブラリ** のとなりのラジオボタンを選択します。
2. **サブスクリプション** の下に次の URL を入力します: https://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json 
3. **認証の有効化** のとなりのチェックボックスは **チェックしない** ままとします。
4. **今すぐ** となっていることを確認します。
5. **Next** をクリックして次に進みます。

![SDDC017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc017.jpg)

1. コンテンツ ライブラリのストレージとして **WorkloadDatastore** をクリックします。
2. **Next** ボタンをクリックします。

![SDDC018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc018.jpg)

1. **Finish** ボタンをクリックします。

**注意: テンプレートの数とサイズによって、コンテンツの同期に時間が掛かります。このコンテンツ ライブラリは同期に数分しかかかりません。**

<!--
## Create Linux Customization Specification

When you clone a virtual machine or deploy a virtual machine from a template, you can customize the guest operating system of the virtual machine to change properties such as the computer name, network settings, and license settings.

Customizing guest operating systems can help prevent conflicts that can result if virtual machines with identical settings are deployed, such as conflicts due to duplicate computer names.

You can specify the customization settings by launching the Guest Customization wizard during the cloning or deployment process. Alternatively, you can create customization specifications, which are customization settings stored in the vCenter Server database. During the cloning or deployment process, you can select a customization specification to apply to the new virtual machine.

Use the Customization Specification Manager to manage customization specifications you create with the Guest Customization wizard.
-->

## Linux カスタマイズ仕様の作成

仮想マシンをクローンする時、またはテンプレートから仮想マシンをデプロイする時に、コンピューター名、ネットワーク設定、ライセンス設定などのプロパティを変更するために、仮想マシンのゲスト オペレーティング システムをカスタマイズすることができます。

ゲスト オペレーティング システムをカスタマイズすることで、同一の仮想マシンをデプロイするといったようなコンピューター名の衝突を避けることができます。

カスタマイズ仕様の設定は、クローンまたはデプロイ プロセスの途中にゲスト カスタマイゼーション ウィザードを起動することで設定できます。また、その代わりとしてカスタマイズ仕様を作成することもできます。これは vCenter のデータベースに保存されます。クローンまたはデプロイ プロセスの中で、新しい仮想マシンに適用するカスタマイズ仕様を選択することができます。

それでは、ゲスト カスタマイゼーション ウィザードを使ってカスタマイズ仕様マネージャーでカスタマイズ仕様を作成しましょう。

<!--
### Navigate to Customization Specifications

![SDDC019](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc019.jpg)

1. Click **Menu**.
2. click on **Policies and Profiles**.
-->

### カスタマイズ仕様の表示

![SDDC019](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc019.jpg)

1. **メニュー** をクリックします。
2. **ポリシーおよびプロファイル** をクリックします。

<!--
### Add a new VM Customization Specification

![SDDC020](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc020.jpg)

1. Click on **+ New** to add a new Linux Customization Specification.
-->

### 新しい仮想マシン カスタマイズ仕様の作成

![SDDC020](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc020.jpg)

1. Linux カスタマイズ仕様を作成するために **+ 新規** をクリックします。

<!--
### Define Customization Specification Details

![SDDC021](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc021.jpg)

1. Enter a **Name** for the Linux Customization Specification (**LinuxSpec** in this example).
2. Optionally enter a **Description**.
3. Select the radio button for **Linux** next to **Target guest OS**.
4. Click the **Next** button to continue.
-->

### カスタマイズ仕様の詳細を定義

![SDDC021](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc021.jpg)

1. Linux カスタマイズ仕様の **名前** を入力します。(この例では **LinuxSpec** とします).
2. 任意に **説明** を入力します。
3. **ターゲット ゲスト OS** のとなりの **Linux** ラジオボタンを選択します。
4. 次に進むために **Next** ボタンをクリックします。

<!--
### Define Specification Naming Standard

![SDDC022](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc022.jpg)

1. Click the **radio button** next to **Use the virtual machine name**.
2. For **Domain name** enter **corp.local**.
3. Click the **Next** button to continue.
-->

### 仮想マシンの命名標準の定義

![SDDC022](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc022.jpg)

1. **仮想マシン名を使用** のとなりの **ラジオボタン** をクリックします。
2. **ドメイン名** には **corp.local** と入力します。
3. 次に進むために **Next** をクリックします。

<!--
### Select Time Zone

![SDDC023](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc023.jpg)

1. Select the appropriate **Area** by clicking on the arrow next to the dropdown field.
2. Select the appropriate **Location**.
3. Click the **Next** button to continue.
-->

### タイムゾーンの選択

![SDDC023](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc023.jpg)

1. ドロップ ダウン フィールドのとなりにある矢印をクリックし、適切な **エリア** を選択します。
2. **場所** 適切な場所を選択します。
3. 次に進むために **Next** をクリックします。

<!--
### Select Network Settings

![SDDC024](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc024.jpg)

1. Ensure the radio button next to **Use standard network settings for the guest operating system. including enabling DHCP in all network interfaces** is selected.
2. Click **Next** to continue.
-->

### ネットワーク設定の選択

![SDDC024](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc024.jpg)

1. **ゲスト OS に標準ネットワーク設定を使用します（すべてのネットワーク インターフェイスで DHCP を有効化など）** のとなりにあるラジオボタンが選択されていることを確認します。
2. 次に進むために **Next** をクリックします。

<!--
### Enter DNS Settings

![SDDC025](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc025.jpg)

1. Enter **8.8.8.8** for the Primary DNS server.
2. Enter **8.8.4.4** for the Secondary DNS server.
3. For the DNS Search paths enter **corp.local**.
4. Click the **Add** button to add the corp.local domain to the DNS search path.
5. Click **Next** to continue.
-->

### DNS 設定の入力

![SDDC025](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc025.jpg)

1. プライマリ DNS サーバーに **8.8.8.8** を入力します。
2. セカンダリ DNS サーバーに **8.8.4.4** を入力します。
3. DNS 検索パスに **corp.local** を入力します。
4. corp.local ドメインを DNS 検索パスに追加するために **追加** ボタンをクリックします。
5. Click **Next** to continue. 次に進むために **Next** をクリックします。

<!--
### Finish Creating the Customization Spec

![SDDC026](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc026.jpg)

1. Review your entries and click on the **Finish** button.
-->

### カスタマイズ使用の作成の終了

![SDDC026](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc026.jpg)

1. 入力した内容を確認し **Finish** ボタンをクリックします。

<!--
### Customization Spec Created

![SDDC027](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc027.jpg)

Congratulations!  You have successfully created your VM Customization Spec for your Linux VM's.  You can also Export (Duplicate), Edit, Import, and Export a VM Customization Spec.
-->

### カスタマイズ使用の作成完了

![SDDC027](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc027.jpg)

おめでとうございます！ Linux 仮想マシン向けの仮想マシン カスタマイズ使用の作成に成功しました。カスタマイズ使用は、エクスポート（複製）、編集、インポートすることができます。

<!--
## Deploy a Virtual Machine

![SDDC-deploy-vm-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

In the vSphere client window already opened, deploy a template from the content library:

1. Click **Menu**.
2. Click on **Content Libraries**.
-->

## 仮想マシンのデプロイ

![SDDC-deploy-vm-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

既にオープンされている vSphere Client のウィンドウで、コンテンツ ライブラリからテンプレートをデプロイします:

1. **メニュー** をクリックします。
2. **コンテンツ ライブラリ** をクリックします。

<!--
### Select Content Library

![SDDC-deploy-vm-028](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc028.jpg)

1.  Click on the **VMC Content Library** that was previously synchronized.
-->

### コンテンツ ライブラリの選択

![SDDC-deploy-vm-028](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc028.jpg)

1.  前の手順で同期した **VMC Content Library** をクリックしてください。

<!--
### Deploy a New Virtual Machine from Template

![SDDC-deploy-vm-029](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc029.jpg)

1. Click the **Templates** tab to access the template synchronized in the content library.
2. Right-click on the **photoapp-u**  template to exporthe Actions menu.
3. Click on **New VM from This Template** to deploy a virtual machine from template.
-->

### テンプレートからの新しい仮想マシンのデプロイ

**重要** 2019年5月21日現在、HTML 5 ベースの vSphere Client において、コンテンツライブラリからのテンプレートの展開に問題が発生しています。ワークアラウンドとして、Adobe Flash ベースの vSphere Client を利用してください。Adobe Flash ベースの vSphere Client の UI は、以下のアドレスにアクセスします。

https://${cloud_vcenter_hostname}/vsphere-client/

${cloud_vcenter_hostname} は HTML 5 ベースの vSphere Client の URL と同じ値が利用されます。

![SDDC-deploy-vm-029](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc029.jpg)

1. コンテンツ ライブラリ内の同期されたテンプレートにアクセスするために、**テンプレート** タブをクリックします。
2. アクション メニューを表示するために **photoapp-u** テンプレートを右クリックします。
3. テンプレートから仮想マシンをデプロイするために **このテンプレートから仮想マシンを新規作成** をクリックします。

<!--
### Choose Virtual Machine Name and Location

![SDDC-deploy-vm-030](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc030.jpg)

1. Enter **webserver01** for the virtual machine name.
2. Click the **arrow** next to SDDC-Datacenter to expose the folders available.
3. In VMware Cloud on AWS customer workloadds should be placed in the Workloads folder (or subfolder).  Click the **Workloads** folder.
4. Select the **Checkbox** next to  **Customize the operating system**.
5. Click **Next** to continue.
-->

### 仮想マシンの名前と場所の選択

![SDDC-deploy-vm-030](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc030.jpg)

1. 仮想マシンの名前として **webserver01** を入力します。
2. SDDC-DataCenter のとなりの **矢印** をクリックし、フォルダー配下を表示します。
3. VMware Cloud on AWS では、お客様のワークロードは ワークロード フォルダー（あるいはそのサブフォルダー）に置かれなければなりません。**Workloads** フォルダーをクリックします。
4. **オペレーティング システムのカスタマイズ** のとなりの **チェックボックス** を選択します。
5. 次に進むために **Next** をクリックします。

<!--
### Choose Virtual Machine Customization Specification

![SDDC-deploy-vm-031](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc031.jpg)

We will utilize the customization specification created in a previous module to customize the operating system.

1. Click to select the **LinuxSpec** customization specification.
2. Click **next** to continue.
-->

### 仮想マシン カスタマイズ仕様を選択

![SDDC-deploy-vm-031](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc031.jpg)

オペレーティング システムをカスタマイズするために、前の項目で作成したカスタマイズ仕様を使います。

1. **LinuxSpec** カスタマイズ仕様をクリックします。
2. 次に進むために **Next** をクリックします。

<!--
### Select Resource Pool

![SDDC-deploy-vm-032](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc032.jpg)

1. Click the **arrow** next to Cluster-1 to expose the resource pools available.
2. In VMware Cloud on AWS customer workloads should be placed in the **Compute-ResourcePool** (or subpool).  Click **Compute-ResourcePool**.
3. Click **Next** to continue.
-->

### リソースプールを選択します。

![SDDC-deploy-vm-032](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc032.jpg)

1. Cluster-1 のとなりの **矢印** をクリックし、配下のリソースプールを表示します。
2. VMware Cloud on AWS では、お客様のワークロードは **Compute-ResourcePool** （もしくはそのサブ リソースプール）に配置されなければなりません。**Compute-ResourcePool** を選択します。
3. 次に進むために **Next** をクリックします。

<!--
### Review the Template Details

![SDDC-deploy-vm-033](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc033.jpg)

Review the details of the template to be deployed.  There may be a security warning displayed, but you can safely ignore that the purpose of this lab.

1. Click **Next** to continue.
-->

### テンプレートの詳細の確認

![SDDC-deploy-vm-033](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc033.jpg)

デプロイされるテンプレートの詳細を確認します。セキュリティの警告が表示されるかも知れませんが、このラボでは無視して問題ありません。

1. 次に進めむために、**Next** をクリックします。

<!--
### Select Storage

![SDDC-deploy-vm-034](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc034.jpg)

Each VMware Cloud on AWS SDDC will include two datastores in order to separate management and customer workloads.  All customer workloads should be placed in the datastore named WorkloadDatastore.

1. Click **workloadDatastore** to select the datstore where the virtual machine will be provisioned.
2. Click **Next** to continue.
-->

### ストレージの選択

![SDDC-deploy-vm-034](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc034.jpg)

VMware Cloud on AWS は、管理ワークロードとお客様のワークロードを分離するために 2 つのデータストアを持ちます。全てのお客様のワークロードは、WorkloadDatastore という名前のデータストアに配置されなければなりません。

1. 仮想マシンがプロビジョニングされるデータストアを選択するために **WorkloadDatastore** をクリックします。
2. 次に進むために **Next** をクリックします。

<!--
### Select the Network for the Virtual Machine

![SDDC-deploy-vm-035](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc035.jpg)

We will use the logical network created in a previous exercise for these virtual machines.

1. Click the **arrow** below Destination Network to select the network for the virtual machine.
2. Click **Demo-Net** to select the network previously created.
3. Click **Next** to continue.
-->

### 仮想マシンのネットワークを選択します

![SDDC-deploy-vm-035](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc035.jpg)

仮想マシンのために、前の項目で作成した論理ネットワークを使います。

1. 仮想マシンのネットワークを選択するために、Click the **arrow** below Destination Network to select the network for the virtual machine.
2. 前の項目で作成したネットワークを選択するため、**Demo-Net** をクリックします。
3. 次に進むために **Next** をクリックします。

<!--
### Complete the Virtual Machine Deployment

![SDDC-deploy-vm-036](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc036.jpg)

1. Review the information for accuracy and click **Finish** to deploy the virtual machine

It should take a couple of minutes for the virtual machine to deploy.  Continue to the next exercise to clone this virtual machine in order to create a second webserver.
-->

### 仮想マシンのデプロイの完了

![SDDC-deploy-vm-036](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc036.jpg)

1. ここまで入力した情報を確認し、仮想マシンをデプロイするために **Finish** をクリックします。

仮想マシンのデプロイが終わるまでに数分かかります。次の項目では、2 つ目の Web サーバーを作成するためにこの仮想マシンのクローンします。

<!--
##  Clone a Virtual Machine

In this exercise, you will clone the virtual machine created in the previous exercise in order to create a second webserver.
-->

##  仮想マシンのクローン

この項目では、2 つ目の Web サーバーを作成するために前の項目で作成した仮想マシンをクローンします。

<!--
### Navigate to VMs and Templates

![SDDC-clone-vm-037](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc037.jpg)

1. Validate the virtual machine deployment completed in the previous exercise by looking for the **Deploy OVF Template** task and verifying it is **Complete**.
2. If complete, click on **Menu**.
3. Click **VMs and Templates** to navigate to the VMs and Templates view.
-->

### 仮想マシンとテンプレートの表示

![SDDC-clone-vm-037](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc037.jpg)

1. 前の項目の仮想マシンのデプロイが完了したことを確認するために、**OVF テンプレートのデプロイ** タスクを探し、それが **完了** していることを確認します。
2. 完了しているならば、**メニュー** をクリックします。
3. **仮想マシンとテンプレート** をクリックして、仮想マシンとテンプレートのビューを表示してします。

<!--
### Select and Power On Webserver01

![SDDC-clone-vm-038](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc038.jpg)

Before we can clone the web server, we will first need to power the VM on so the cutomization specification can execute:

1. Click the **arrow** next to **SDDC-Datacenter** to expose the sub-folders.
2. Click the **arrow** next to workloads to expose **webserver01**
3. Click on the virtual machine **webserver01**
4. Click the **green arrow** in the top center of the screen to execte the power on operation.

**Note: Please wait until the virtual machine is fully powered on before proceeding to the next step.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> If the webserver doesn't connect to the network and does not receive and IP address from DHCP, ensure the NIC is connected by right-clicking on <b>webserver01</b> and then <b>Edit Settings</b> and make sure the checkbox next to Connected is selected.  You may need to repeat this step for the cloned VM webserver02.
</font>
</aside>
-->

### Webserver01 のパワーオン

![SDDC-clone-vm-038](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc038.jpg)

Web サーバーをクローンする前に、カスタマイズ仕様が実行されるように、まず最初に仮想マシンを起動します。

1. サブフォルダーを表示するために **SDDC-Datacenter** のとなりの **矢印** をクリックします。
2. **webserver01** を表示するために Workloads のとなりの **矢印** をクリックします。
3. 仮想マシン **webserver01** をクリックします。
4. 仮想マシンをパワーオンするために、画面上部中央の **緑の矢印** をクリックします。


**注意: 次の項目に進む前に、仮想マシンが完全にパワーオンされるまでお待ち下さい**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> もし webserver がネットワークに接続されておらず、DHCP から IP アドレスを受け取れていない場合、<b>webserver01</b> を右クリックし、<b>設定の編集</b> をクリックし、 NIC の接続のとなりのチェックボックスが選択されていることを確認し、NIC が接続されていることを確認してください。次の項目でクローンした webserver02 でもこの作業が必要になるかも知れません。
</font>
</aside>

<!--
### Initiate Cloning of the Virtual Machine

![SDDC-clone-vm-039](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc039.jpg)

We will now begin the process of cloning this virtual machine.

1. Right-click on **webserver01** to expose the Actions menu.
2. Click on **Clone** to expose a secondary menu of options.
3. Click **Clone to Virtual Machine** to initiate the cloning wizard.
-->

### 仮想マシンのクローン実行

![SDDC-clone-vm-039](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc039.jpg)

それでは仮想マシンのクローンを行いましょう。

1. アクション メニューを表示するために **webserver01** を右クリックします。
2. セカンダリ メニューを表示するために **クローン作成** をクリックします。
3. クローン ウィザードを起動するために **仮想マシンにクローン作成** をクリックします。

<!--
### Select Virtual Machine Name and Folder

![SDDC-clone-vm-040](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc040.jpg)

1. Next to **Virtual machine name** enter **webserver02**.
2. Click the **Workloads** folder for the virtual machine location.
3. Click **Next** to continue.
-->

### 仮想マシンの命名とフォルダーの選択

![SDDC-clone-vm-040](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc040.jpg)

1. **仮想マシン名** のとなりに **webserver02** と入力します。
2. 仮想マシンの配置場所として **Workloads** フォルダをクリックします。
3. 次に進むために **Next** をクリックします。

<!--
### Select Virtual Machine Compute Resource

![SDDC-clone-vm-041](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc041.jpg)

1. Click on **Compute-ResourcePool** to ensure it is selected for the target virtual machine.
2. Click **Next** to continue.
-->

### 仮想マシンのコンピュート リソースを選択

![SDDC-clone-vm-041](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc041.jpg)

1. 対象となる仮想マシンのために **Compute-ResourcePool** をクリックします。
2. 次に進むために **Next** をクリックします。

<!--
### Select Virtual Machine Datastore

![SDDC-clone-vm-042](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc042.jpg)

1. Click on **WorkloadDatastore** to ensure it is selected as the destination for the virtual machine.
2. Click **Next** to continue.
-->

### 仮想マシン データストアの選択

![SDDC-clone-vm-042](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc042.jpg)

1. 対象となる仮想マシンのために **WorkloadDatastore** をクリックします。
2. 次に進むために **Next** をクリックします。

<!--
### Select Clonging Options

![SDDC-clone-vm-043](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc043.jpg)

We will now set the options for cloning.  We will need to customize the operating system to change the server name and als power on the virtual machine after cloning is complete.

1. Click the checkbox next to **Customize the operating system**.
2. Click the checkbox next to **Power on virtual machine after creation**.
3. Click **Next** to continue.
-->

### クローン オプションの選択

![SDDC-clone-vm-043](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc043.jpg)

ここではクローン作成時のオプションを設定します。サーバー名の変更といったオペレーティング システムのカスタマイズや、クローン作成完了時に仮想マシンをパワーオンするには必要となります。

1. **オペレーティング システムのカスタマイズ** のとなりのチェックボックスをクリックします。
2. **作成後に仮想マシンをパワーオン** のとなりのチェックボックスをクリックします。
3. 次に進むために **Next** をクリックします。

<!--
### Choose Virtual Machine Customization Specification

![SDDC-clone-vm-044](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc044.jpg)

We will utilize the customization specification created in a previous exercise to customize the operating system.

1. Click to select the **LinuxSpec** customization specification.
2. Click **Next** to continue.
-->

### 仮想マシン カスタマイズ仕様の選択

![SDDC-clone-vm-044](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc044.jpg)

オペレーティング システムをカスタマイズするために前の項目で作成したカスタマイズ仕様を利用することができます。

1. カスタマイズ仕様 **LinuxSpec** を選択するためにクリックします。
2. 次に進むために **Next** をクリックします。

<!--
### Complete the Virtual Machine Deployment

![SDDC-clone-vm-045](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc045.jpg)

1. Review the information for accuracy and click **Finish** to clone the virtual machine.

It should take a couple of minutes fort the virtual machine to clone.  Continue to the next exercise to learn about securing workloads in VMware Cloud on AWS.

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> If the webserver doesn't connect to the network and does not receive and IP address from DHCP, ensure the NIC is connected by right-clicking on <b>webserver01</b> and then <b>Edit Settings</b> and make sure the checkbox next to Connected is selected.  You may need to repeat this step for the cloned VM webserver02.
</font>
</aside>
-->

### 仮想マシンのデプロイの完了

![SDDC-clone-vm-045](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc045.jpg)

1. 入力した情報を確認し、仮想マシンをクローンするために **Finish** をクリックします。

仮想マシンをクローンするには 2−3 分程度掛かります。以降の項目では、VMware Cloud on AWS でワークロードを安全に保護する方法を学びます。

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> もし webserver がネットワークに接続されておらず、DHCP から IP アドレスを受け取れていない場合、<b>webserver01</b> を右クリックし、<b>設定の編集</b> をクリックし、 NIC の接続のとなりのチェックボックスが選択されていることを確認し、NIC が接続されていることを確認してください。次の項目でクローンした webserver02 でもこの作業が必要になるかも知れません。
</font>
</aside>

<!--
## Testing connectivity between the Virtual Machines

In this exercise we will test the connectivity between webserver01 and webserver02, which we created in the previous exercises.
-->

## 仮想マシン間の接続性テスト

この項目では、前の項目で作成した webserver01 と webserver02 間の接続性テストを行います。

<!--
### Open Console to Webserver01

![SDDC-test-vm-046](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc046.jpg)

We need to open a console session to webserver01 to validate it can communicate with webserver02.

1. In the vSphere Web Client click on Webserver01 to bring it into focus.
2. Click the black box below Summary in the middle of the screen. This will attempt to launch a console session but it may fail because the pop-up was blocked. If this occurs follow steps 3-6, otherwise proceed to the next section.
3. Click the icon with the small red x in the Chrome address bar to launch to pop-up blocker dialog.
4. Click the radio button next to Always allow pop-ips from https://vcenter.sddc-xx-xx-xxxx.vmwarevmc.com
5. Clock the Done button.
6. Return to the black box below the Summary and click it again. The console session should launch in a new tab.
-->

### Webserver01 のコンソール

![SDDC-test-vm-046](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc046.jpg)

webserver02 に通信できることを確認するために、webserver01 へのコンソール セッションを開きます。

1. vSphere Client にて、webserver01 を選択済みにするため、webserver01 をクリックします。
2. サマリタブの画面中程にある黒い四角をクリックします。この操作は、新しいコンソール セッションを開こうとしますが、もしかするとポップアップがブロックされ開けないかも知れません。その場合は、以下の 3−6 のステップを実行し、そうでなければ次の項目に進みます。
3. ポップアップ ブロッカー ダイアログを開くために Chrome のアドレス バーにある小さな赤い X マークのアイコンをクリックします。
4. 「https://vcenter.sddc-xx-xx-xxxx.vmwarevmc.com のポップアップを常に許可する」のとなりのラジオボタンをクリックします。
5. 完了をクリックします。
6. サマリタブに戻り黒い四角をもう一度クリックします。こんどは新しいタブでコンソール セッションが開くはずです。

<!--
### Find the IP Address for Webserver02

![SDDC-test-vm-047](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc047.jpg)

Before we can test connectivity between the two servers, we need to find the IP address of webserver02.

1. Click the **Chrome Tab** of the vSphere Web Client to bring it back into focus.
2. Click on the virtual machine **webserver02**.
3. Take note of the **IP Address** for webserver02 in the middle of the screen. This will be needed in the next step.
4. Click the **Chrome Tab** of the console session for webserver01 to bring it back into focus.
-->

### Webserver02 の IP アドレスの確認

![SDDC-test-vm-047](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc047.jpg)

2 つのサーバー間の接続性をテストする前に、webserver02 の IP アドレスを確認する必要があります。

1. フォーカスを戻すために、vSphere Client (HTML5) の **Chrome Tab** をクリックします。
2. 仮想マシン **webserver02** をクリックします。
3. 画面中程の webserver02 の **IP アドレス** をメモします。これは以降の項目で必要となります。
4. フォーカスを戻すために、webserver01 のコンソール セッションの **Chrome Tab** をクリックします。

<!--
### Login and Ping Webserver02

![SDDC-test-vm-048](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc048.jpg)

Now that we have to IP address for Webserver02 let's setup a continuous ping to the server to verify communication.

Before beginning click anywhere inside the console window to bring it into focus

1. At the login prompt enter **root** and press Enter.
2. At the password prompt enter **VMware1!** and press Enter.
3. At the console prompt, enter **ping 10.10.xx.xxx** and press Enter.  The third octet is based on student number and the last octet of the IP address in most cases it will be 101, but verify this in your configuration.
4. Verify the pings are successful.

**NOTE: Please leave this ping and console Window open for the next lesson. We will revisit it to verify the web servers can no longer communicate.**

Congratulations! You have now deployed two web servers in VMware Cloud on AWS SDDC and verified they can communicate with each other. In the next lesson we will create firewall rules to block the servers from communicating with each other and also make webserver02 accessible from the internet.
-->

### ログインと webserver02 への ping

![SDDC-test-vm-048](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc048.jpg)

webserver02 の IP アドレスが確認できましたので、接続性を確認するために ping を実行しましょう。

作業を始める前に、フォーカスをコンソール ウィンドウに合わせるためコンソール ウィンドウのどこかをクリックします。

1. ログイン プロンプトで **root** と入力し、Enter を押下します。
2. パスワード プロンプトで **VMware1!** と入力し、Enter を押下します。
3. コンソール プロンプトで、**ping 10.10.xx.xxx** と入力し Enter を押下します。3 番目のオクテットは受講者番号を、最後のオクテットは大体の場合 101 となります。異なる場合は構成を確認してください。
4. ping が成功していることを確認してください。

**注意: 次のレッスンで利用するため、コンソール ウィンドウは開いたままとし、ping も実行し続けておいて下さい。web サーバーが通信できなくなることをこの後確認することになります。**

おめでとうございます。これで VMware Cloud on AWS 上に 2 つの Web サーバーがデプロイされ、お互いが通信できることを確認できました。次のレッスンでは、サーバー間のお互いの通信をブロックするファイアウォール ルールを作成します

<!-- 訳注: webserver02 を インターネットからアクセスできるようにする設定はこのモジュールには含まれていないため、記述を削除 -->

<!--
## Configuring VMware Cloud on AWS Advanced Network Services

VMware Cloud on AWS Advanced Network Services is now available for new SDDC deployments.
-->

## VMware Cloud on AWS アドバンスド ネットワーク サービスの構成

新しくデプロイされた SDDC では、VMware Cloud on AWS アドバンスド ネットワーク サービスが利用できます。

<!--
### Distributed Firewall in VMware Cloud on AWS Advanced Network Services

![DFW-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw01.jpg)

Using VMware Cloud on AWS Advanced Network Services, users have the capability to implement micro-segmentation with Distributed Firewall. Granular security policies can be
applied at the VM-level allowing for segmentation within the same L2 network or across separate L3 networks. This is shown in the diagram above.

All networking and security configuration is now done through the VMware Cloud on AWS console via the Networking & Security tab, including creating network segments. This provides ease of operations and management by having all networking and security access through the console.
-->

### VMware Cloud on AWS アドバンスド ネットワーク サービスにおける分散ファイアウォール

![DFW-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw01.jpg)

VMware Cloud on AWS のアドバンスド ネットワーク サービスを使うことで、分散ファイアウォールを使ってマイクロセグメンテーションを実装することができるようになります。粒度の細かいセキュリティ ポリシーを仮想マシンレベルに適用することができます。このポリシーでは、同じ L2 ネットワークや L3 を跨いだセグメンテーションを可能とします。これは上の図のとおりとなります。

全てのネットワークとセキュリティの構成は、VMware Cloud on AWS コンソールのネットワークとセキュリティ タブを通して行われます。ネットワークセグメントの作成もこの UI を通して行います。コンソールからネットワークとセキュリティの全てにアクセスできることで運用と管理の簡便さを提供しています。

<!--
### Distributed Firewall

![DFW-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw02.jpg)

From the above screenshot, you can see, in addition to the ability to create multiple sections, users can organize Distributed Firewall rules into groups (Emergency Rules, Infrastructure Rules, Environment Rules, and Application Rules. The rules are hit from the top-down.
-->

### 分散ファイアウォール

![DFW-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw02.jpg)

上のスクリーンショットで確認できるように、複数のセクションを作成できるだけでなく、ユーザーは分散ファイアウォール ルールをグループ（緊急時ルール、インフラストラクチャ ルール、環境ルール、アプリケーションルール）に整理することができます。ルールは上から下に適用されます。

<!--
### Security Groups

![DFW-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw03.jpg)

In addition to the new Distributed Firewall capabilities, grouping objects can now be leveraged within security policies. Security groups support the following grouping criteria/constructs:

* IP Address
* VM Instance
* Matching criteria of VM Name
* Matching Criteria of Security Tag

As shown above, Security Groups can be created under Workload Groups or Management Groups. Workload Groups can be used in DFW and CGW firewall policies and Management Groups can be used under MGW firewall policies. Management Groups only support IP addresses as these groups are infrastructure based. Predefined Management Groups groups already exist for vCenter, ESXi hosts, and NSX Manager. Users can also create groups here based on IP address for on-prem ESXi hosts, vCenter, and other management appliances.
-->

### セキュリティ グループ

![DFW-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw03.jpg)

分散ファイアウォール機能に加えて、セキュリティ ポリシーでオブジェクトをグループ化する機能を利用できます。セキュリティグループは、以下のグルーピングをサポートしています。

* IP アドレス
* VM インスタンス
* 仮想マシン名とのマッチング基準
* セキュリティ タグとのマッチング基準

上にあるように、セキュリティ グループはワークロード グループと管理グループの配下に作成されます。ワークロードグループは、分散ファイアウォールと CGW のファイアウォール ポリシーに使われ、管理グループは MGW ファイアウォール ポリシーに使われます。管理グループはインフラベースとなるため、IP アドレスのみをサポートします。定義済みの管理グループのグループとして、vCenter、ESXi ホスト、NSX Manager があります。ユーザーは、オンプレミスの ESXi ホスト、vCenter、その他の管理アプライアンスのために IP アドレスベースのグループを個々に追加することができます。

<!--
### View VM's in a Security Group

![DFW-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw04.jpg)

Here you can see we have deployed some VMs in vCenter and you can see the VMs in inventory within the console. Additionally, we have tagged the VMs with Web, App, and DB Security Tags respectively.
-->

### セキュリティ グループでの仮想マシンの確認

![DFW-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw04.jpg)

vCenter でデプロイした仮想マシンを確認することもできますが、コンソールのインベントリでも仮想マシンを確認することができます。加えて、各々 Web、App、DB といったセキュリティタグが仮想マシンにタグ付けされています。

<!--
### Tagging Virtual Machines

![DFW-tag-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw05.jpg)

Through the VMware Cloud on AWS console we can apply security tags to virtual machines and then group them.

We will now switch back to the VMware Cloud on AWS console.

1. Click on the **VMware Cloud on AWS Chrome tab** and login with the information you were provided if your session has expired.
2. Click on **View Details** to access the details for the SDDC.
-->

## 仮想マシンのタグ付け

![DFW-tag-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw05.jpg)

VMware Cloud on AWS コンソールにて、仮想マシンにセキュリティ タグを適用し、そしてこれをグルーピングに用います。

VMware Cloud on AWS のコンソールに戻りましょう。

1. **VMware Cloud on AWS タブ** をクリックします。もしセッションが期限切れとなっている場合には、ログインし直してください。
2. SDDC の詳細にアクセスするため **詳細** をクリックします。

<!--
### Edit Tags for Webserver01

We will now begin tagging the virtual machines with security tags.

![DFW-tag-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw06.jpg)

1. Click on the **Networking & Securit**y tab to access the networking configuration.
2. On the left-hand side of the screen click on **Groups**.
3. Under Groups, click on **Virtual Machines** to access the virtual machines that are part of the SDDC.
4. Locate **webserver01** and click the three vertical dots and click Edit.
-->

### Webserver01 のタグ編集

ここでは仮想マシンにセキュリティ タグをタグ付けします。

![DFW-tag-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw06.jpg)

1. ネットワーク構成を変更するために **ネットワークとセキュリティ** タブをクリックします。
2. 画面左の **グループ** をクリックします。
3. グループの下部、SDDC の一部の仮想マシンにアクセスするために **仮想マシン** をクリックします。
4. **webserver01** の横にある縦に並んでいる 3 つの点をクリックし、続いて編集をクリックします。

<!--
### Add Security Tag for Web

![DFW-tag-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw07.jpg)

1. Under Tags, enter **Web** for webserver01.
2. Click **Save** to commit the changes.
-->

### Web サーバーのセキュリティ タグの追加

![DFW-tag-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw07.jpg)

1. タグの下に webserver01 のためのタグ **Web** を入力します。
2. 変更を反映するために **Save** をクリックします。

<!--
### Edit Tags for Webserver02

![DFW-tag-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw08.jpg)

We will now tag Webserver02 with the same Web tag. We will use this to create a group for both web servers.

1. Locate **webserver02** and click the three vertical dots and click **Edit**.
-->

### Webserver02 のタグ編集

![DFW-tag-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw08.jpg)

次に Webserver02 に同じ Web タグをタグ付けします。このタグにより両方の Web Server のためのグループを作成します。

1. **webserver02** の横にある縦に並んでいる 3 つの点をクリックし、**編集** をクリックします。

<!--
### Add Security Tag for Webserver02

![DFW-tag-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw09.jpg)

1. Under Tags, Enter **Web** for webserver02.
2. Click **Save** to commit the changes.
-->

### Webserver02 のセキュリティ タグの追加

![DFW-tag-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw09.jpg)

1. タグの下に webserver02 のためのタグ **Web** を入力します。
2. 変更を反映するために **Save** をクリックします。

<!--
### Creating a Dynamic Group

![DFW-tag-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw010.jpg)

Groups can be used in VMware Cloud on AWS Advanced Network Services to group virtual machines and simplify rulebase configuration. In this exercise we will group the two webservers into a group and then create a firewall rule to block communication between them. In a properly architected traditional application there is usually no need for servers in the web tier to communicate.

We will now create a group of web servers based on the dynamic security tag we applied earlier.

1. Click on **Workload Groups**.
2. Click on **Add Group**.
3. Under Name enter **Web** for the name of the group.
4. Under Member Type, click the **drop down** and select **Membership Criteria**.
5. Under Members click **Set Membership Criteria**.
-->

### 動的グループの作成

![DFW-tag-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw010.jpg)

VMware Cloud on AWS のアドバンスド ネットワーク サービスにおいて、グループは、仮想マシンをグループ化し、ルールの構成を単純化するために利用されます。この演習では、2 つの Web サーバーを一つのグループにし、これらの間の通信をブロックするファイアウォール ルールを作成します。適切に設計された従来型のアプリケーションでは、Web 層のサーバー同士は通信する必要がありません。

ここでは、前の項目で作成した動的なセキュリティ タグを基にして Web サーバーのグループを作成します。

1. **ワークロード グループ** をクリックします。
2. **グループの追加** をクリックします。
3. 名前の下に、グループの名前となる **Web** を入力します。
4. メンバーのタイプの下の **ドロップダウン** をクリックし **Membership Criteria** を選択します。
5. メンバーの下の **メンバーシップ基準の設定** をクリックします。

<!--
### Add Membership Criteria

![DFW-tag-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw011.jpg)

We will now add the criteria to group machines based on security tag.

1. Click on **+ Add Criteria**.
2. Under Property, click the **drop-down** and select **Tag**.
3. Under Value, enter **Web**.
4. Click **Save** to continue.
-->

### メンバーシップ基準の追加

![DFW-tag-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw011.jpg)

ここではセキュリティ タグを基にして、仮想マシンをグループ化する基準を追加します。

1. **+ Add Criteria** をクリックします。
2. Property の下の **ドロップダウン** をクリックし、**タグ** を選択します。
3. Value の下で **Web** を選択します。
4. 次に進むために **Save** をクリックします。

<!--
### Save Workload Group Changes

![DFW-tag-012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw012.jpg)

1. Click **Save** to commit the changes.
-->

### ワークロード グループの変更の保存

![DFW-tag-012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw012.jpg)

1. 変更を反映するために **保存** をクリックします。

<!--
### View Members

![DFW-tag-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw013.jpg)

We can now validate the group membership is working as expected.

1. Click the **three vertical** dots next to the Web group.
2. Click on **View Members** to show the current members of the dynamic group.
-->

### メンバーの表示

![DFW-tag-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw013.jpg)

ここでは、グループ メンバーシップが期待通りに機能しているかどうかを確認します。

1. Web グループ横の **縦に 3 つ並んだ点** をクリックします。
2. 動的グループの現在のメンバーを表示するために **メンバーの表示** をクリックします。

<!--
### Validate Group Members

![DFW-tag-014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw014.jpg)

1. Validate that both **webserver01** and **webserver02** appear in the group membership. If they do not, go back and verify there are no typos.
2. Click **Close**.

Now that this group is created, you can easily add new members by simply applying a security tag.
-->

### グループ メンバーの確認

![DFW-tag-014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw014.jpg)

1. グループ メンバーシップに **webserver01** と **webserver02** の両方が含まれていることを確認します。含まれていなかった場合、前の手順に戻り打ち間違えがないかどうかを確認します。
2. **Close** をクリックします。

これでグループが作成されました。セキュリティ シンプルにタグを適用することで、簡単に新しいメンバーを追加することができます。

<!--
### Create a Firewall Rule Section

![DFW-tag-015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw015.jpg)

Now that we have created our dynamic group, let's create a firewall rule to block access between the web servers.

1. Click **Distributed Firewall** on the left-hand side of the screen.
2. Click **Application Rules**.
3. Click **Add New Section** to create a new section for the rule. This functionality allows you to group rules logically to make operating the environment simpler.
4. Under Name, enter **Web Tier**.
5. Click **Publish** to commit the changes.
-->

### ファイアウォール ルール セクションの作成

![DFW-tag-015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw015.jpg)

動的グループが作成されましたので、Web サーバー間のアクセスをブロックするファイアウォール ルールを作成しましょう。

1. 画面左の **分散ファイアウォール** をクリックします。
2. **アプリケーション ルール** をクリックします。
3. ルールのための新しいセクションを作成するため、**新しいセクション** をクリックします。この機能により、環境の運用を簡単にするために論理的にルールをグループ化することができます。
4. 名前の下で **Web Tier** と入力します。
5. 変更を反映するために **発行** をクリックします。

<!--
### Add Firewall Rule

![DFW-tag-016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw016.jpg)

Now that we have the section created, we can now add a firewall rule.

1. Click the **arrow** next to the Web Tier section.
2. Click **Add New Rule** in the menu above the rules.
3. Under Name, enter **Block Web To Web**.
4. Under Action, click the **drop-down** and select **Drop**.
5. Under Sources click **Any**.
-->

### ファイアウォール ルールの追加

![DFW-tag-016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw016.jpg)

セクションが作成されたので、ファイアウォール ルールを作成することができます。

1. Web Tier セクションのとなりの **矢印** をクリックします。
2. ルールの上のメニューから **新しいルールの追加** をクリックします。
3. 名前の下に **Block Web To Web** と入力します。
4. アクションの下で、**ドロップダウン** をクリックし、**ドロップ** を選択します。
5. 送信元の下で、**Any** をクリックします。

<!--
### Select Source

![DFW-tag-017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw017.jpg)

1. Click the **checkbox** next to Web.
2. click **Save** to commit the changes to the rule.
-->

### 送信元の選択

![DFW-tag-017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw017.jpg)

1. Web のとなりの **チェックボックス** をクリックします。
2. ルールの変更を反映するために **Save** をクリックします。

<!--
### Add Destination

![DFW-tag-018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw018.jpg)

1. Under Destinations click **Any**.
-->

### 宛先の追加

![DFW-tag-018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw018.jpg)

1. 宛先の下の **Any** をクリックします。

<!--
### Select Destination

![DFW-tag-019](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw019.jpg)

1. Click the **checkbox** next to Web.
2. click **Save** to commit the changes to the rule.
-->

### 宛先の選択

![DFW-tag-019](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw019.jpg)

1. Web のとなりの **チェックボックス** をクリックします。
2. ルールの変更を反映するために **Save** をクリックします。

<!--
### Publish Firewall Rule

![DFW-tag-020](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw020.jpg)

1. Click **Publish** to commit the rule and begin blocking traffic between the web servers.
-->

### ファイアウォール ルールの発行

![DFW-tag-020](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw020.jpg)

1. ルールを反映するために **発行** をクリックします。このあと Web サーバー間のトラフィックはブロックされます。

<!--
### Testing the Distributed Firewall Rule

![DFW-tag-021](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw021.jpg)

You should still have the console session opened from the previous exercise to webserver01 and it should be running a ping command.
1. Click the Chrome Tab for **webserver01**.
2. Ping webserver02 IP address 10.10.xx.xxx.

The pings should have stopped responding meaning that the distributed firewall rules have been correctly applied. This simple demonstration should give you an idea of the power of the distributed firewall.
-->

### 分散ファイアウォール ルールのテスト

![DFW-tag-021](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw021.jpg)

まだ webserver01 に対する前の演習でコンソールセッションが開いており、ping コマンドが実行されたままのはずです。

1. **webserver01** の Chrome タブをクリックします。
2. webserver02 の IP アドレス 10.10.xx.xxx へ ping します。

ping の反応がなくなっているはずです。これは分散ファイアウォール ルールが正しき適用されていることを意味します。この簡単なデモンストレーションで分散ファイアウォールのメリットをご理解頂けたかと思います。

<!--
## Conclusion

In this module, we explored the setup of configuration of a VMware Cloud on AWS SDDC including utilizing the content library, deploying virtual machines, modifying firewall rules and working with virtual machines.
-->

## まとめ

このモジュールでは、コンテンツライブラリの利用、仮想マシンのデプロイ、ファイアウォールの修正と仮想マシンへの適用といった VMware Cloud on AWS の構成・設定を見てきました。

<!--
## Single Host SDDC

If you like the Lab and want to continue experiment and test the VMware Cloud on AWS capabilities, please scan the QR Code below to start your 1-Host experience.

![DFW-tag-022](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw022.jpg)

You have completed this module.

Please add comments below if you would like to give feedback on this exercise.
-->

## シングル ホスト SDDC

このラボを気に入って頂き、VMware Cloud on AWS の機能をテストされたい場合には、1 ホスト SDDC を開始するために以下の QR コードをスキャンして下さい。

![DFW-tag-022](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw022.jpg)

これでこのラボは完了しました。

この演習にフィードバックを頂ける場合は、以下にコメントをご記入下さい。