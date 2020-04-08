---
layout: single
title: "Hybrid Cloud Extension (HCX) ラボ マニュアル"
categories: labs
date: 2018-10-01
tags: workshop
toc: true
classes: wide
author_profile: false
---
<!-- based on commit: 92156fe1fbcc43df548a5ef8cbe5e20d05ff5413 -->

<!--
# Introduction 

In this lab exercise you will learn about Hybr Cloud Extension (HCX), Primarily this is a tool, bundled with VMware Cloud on AWS, which will allow you to bulk migrate workloads to VMware Cloud on AWS and significantly reduce the time and complexity of moving workloads into the public cloud sphere.
-->

# はじめに

このラボでは Hybrid Cloud Extension (HCX) について学びます。このツールは VMware Cloud on AWS にバンドルされています。VMware Cloud on AWS へのワークロードをバルク マイグレーションをサポートし、パブリック クラウドへのワークロードの移行にまつわる複雑さと移行に掛かる時間を著しく削減することができます。

<!--
## What is Hybrid Cloud Extension (HCX)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX1.jpg)

Hybrid Cloud Extension abstracts on-premises and cloud resources and presents them to the apps as one continuous hybrid cloud. On this, Hybrid Cloud Extension provides high-performance, secure and optimized multisite interconnects. The abstraction and interconnects create infrastructure hybridity. Over this hybridity, Hybrid Cloud Extension facilitates secure and seamless app mobility and disaster recovery across on-premises vSphere platforms and VMware Clouds. Hybrid Cloud Extension is a multi-site, multi cloud service, facilitating true hybrid cloud.
-->

## Hybrid Cloud Extension (HCX) とは何か

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX1.jpg)

Hybrid Cloud Extension は、オンプレミスとクラウドのリソースを抽象化し、一つの連続したハイブリッド クラウドとしてアプリケーションに提供されます。Hybrid Cloud Extension は、ハイパフォーマンスで、安全で、最適化されたマルチサイトのインターコネクトを提供します。この抽象化とインターコネクトにより、インフラストラクチャのハイブリッド性が成されます。Hybrid Cloud Extension は、安全でシームレスなアプリケーションのモビリティと、オンプレミスの vSphere プラットフォームと VMware Cloud 間の災害対策を容易にします。Hybrid Cloud Extension は、マルチ サイトのサービスであり、マルチ クラウドのサービスでもあり、真のハイブリッド クラウドを実現します。

<!--
### Hybrid Cloud Extension Features

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX2.jpg)

Any-to-Any vSphere Cloud App Mobility

• Rapidly move existing workloads from a vSphere platform to the latest SDDC

• Reduce upfront planning time for cost and resource analysis

• Accelerate cloud adoption and avoid retrofitting on-premises environment Business Continuity with Lower TCO

**Business Continuity with Lower TCO**

• IP and MAC address remapping is not required

•  No need to retrofit existing VM environment

• Hybrid Cloud Extension provides warm and cold bulk migration, and bidirectional migration

• Hybrid Cloud Extension simplifies your operational model

**Architected for Security**

• Ensure highly secure tethering of private and public clouds

• Protect resources with resilient disaster recovery capabilities

• Hybrid Cloud Extension hybrid DMZ enables portability of enterprise network and security practices to the cloud

• Security policies migrate with applications High-Performance Infrastructure Hybridity

**High-Performance Infrastructure Hybridity**

• In-built WAN optimization is tuned for the needs of hybrid use cases

• Hybrid Cloud Extension provides agile, intelligent routing

• Traffic load balancing overlay is policy-enforced

• Multiple VM migration models (including vMotion, warm, cold) make it easy to migrate to and from the cloud without any changes
-->

### Hybrid Cloud Extension の機能

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX2.jpg)

- **Any-to-Any の可搬性**
  - クラウド化の準備とアプリケーションの依存関係のアセスメントを不要にします
  - vSphere プラットフォームから最新の SDDC へ既存のワークロードを迅速に移動させます
  - コストやリソースの分析のための事前の計画時間を削減します
  - クラウドへの適用を加速し、オンプレミス環境の改修を避けることができます
- **低い TCO でのビジネス継続性**
  - IP アドレス、MAC アドレスの再設定が不要
  - 既存の仮想マシン環境の改修が不要
  - Hybrid Cloud Extension は、ウォーム バルク マイグレーションとコールド バルク マイグレーションを提供し、さらには双方向のマイグレーションも提供します
  - Hybrid Cloud Extension は運用モデルをシンプルにします
- **セキュリティを踏まえた設計**
  - プライベート クラウドとパブリック クラウドの接続をよりセキュアに実現
  - 回復性のある災害対策機能によるリソースの保護
  - Hybrid Cloud Extension の Hybrid DMZ は、エンタープライズのネットワークの可搬性とクラウドへのセキュリティのプラクティスを可能にします。
  - アプリケーションと共にセキュリティ ポリシーの移行
- **ハイパフォーマンスなインフラストラクチャのハイブリッド性**
  - ハイブリッドなユースケースのニーズのためのビルトインされた WAN 最適化
  - Hybrid Cloud Extension は俊敏性のあるインテリジェントなルーティングを提供
  - トラフィックがロードバランシングされたオーバーレイはポリシーが適用されます
  - 複数の仮想マシンの移行モデル (vMotion、ウォーム、コールド) は変更無しでクラウドから、そしてクラウドへの移行を容易に。

<!--
## HCX - vMotion Migration

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX3.jpg)

1\. Open your Chrome browser and click on the *HCX-vMotion* bookmark

2\. Click the *X* on the right pane to enlarge the main screen The first tab in the browser demonstrate an on-premises vCenter server.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX4.jpg)

3\. Click on the second tab, this represents a second data center (This can also represent a VMware Cloud on AWS vCenter)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX5.jpg)

4\. Click the first tab in the browser to go back to the on-premises vCenter.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX6.jpg)

5\. Click on the VM named *Mission Critical Workload 1*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX7.jpg)

6\. Click on the Console screen

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX8.jpg)

A console window is now open for the *Mission Critical Workload 1* VM, it will try to ping IP Address 10.159.137.212 which corresponds to a VM in the second site.

7\. Click in the tab corresponding to the second site

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX9.jpg)

8\. Click on the VM named *TargetSite-TestVM*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX10.jpg)

9\. Note the IP address corresponding to this VM is the IP address that the *Mission Critical Workload 1* VM on the source site is trying to ping

10\. Click on the *Mission Critical Workload 1* tab

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX11.jpg)

• Press enter after the ping command

• Click *Control-C* to stop ping

• Type **ping 172.16..4.2** which is this VM's own IP address

11\. Click the *X* on this tab to close this tab on your browser

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX12.jpg)

12\. Click on the first tab to go back to the on-premises vCenter

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX13.jpg)

13\. Click on the *Actions* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX14.jpg)

14\. Click on *Hybridity Actions*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX15.jpg)

15\. Click on *Migrate to the Cloud*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX16.jpg)

16\. Click on *(Specify Destination Container)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX17.jpg)

17\. Select *RedwoodCluster*

18\. Click on *Select Destination* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX18.jpg)

19\. Click on *(Select Storage)* button

20\. Select *cloudStorage*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX19.jpg)

21\. Click *(Select Virtual Disk Format)*

22\. Select *Same format as source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX20.jpg)

23\. Click on *(Select Migration Type)*

24\. Select *vMotion*

25\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX21.jpg)

26\. Look for the *Validation is Successful* message

27\. Click *Finish* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX22.jpg)

28\. Click on the *Home* button

29\. Click *HCX* in the left pane

30\. Click the *Migration* tab

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX23.jpg)

31\. Notice the progress of the vMotion migration

32\. Click on the *Refresh* tab to update the progress

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX24.jpg)

33\. Once progress has completed, click on the second tab to open the target site's vCenter

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX25.jpg)

34\. You now see that *Mission Critical Workload 1* has successfully migrated to the Target Site, click on its name


35\. Click on the Console window

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX26.jpg)

• You can see that the ping you left running in a previous step never stopped, successfully vMotioning the VM with zero downtime

• Press *Control-C* to stop the ping

36\. Click on the *X* on the browser tab to close the console tab
-->

## HCX - vMotion マイグレーション

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX3.jpg)

1\. Chrome ブラウザを起動し、 **HCX-vMotion** ブックマークをクリックします。

2\. メイン スクリーンを最大化するために、右上の **-** をクリックします。ブラウザの最初のタブはオンプレミスの vCenter になります

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX4.jpg)

3\. 2 つ目のタブをクリックします。これは 2 つ目のデータセンター (VMware Cloud on AWS の vCenter) になります。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX5.jpg)

4\. ブラウザの最初のタブをクリックし、オンプレミスの vCenter を表示します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX6.jpg)

5\. *Mission Critical Workload 1* という名前の仮想マシンをクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX7.jpg)

6\. *Console screen* をクリックします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX8.jpg)

仮想マシン *Mission Critical Workload 1* のコンソール ウィンドウが開きました。2 つ目のサイトの VM に対応する IP アドレス  10.159.137.212 へ ping を打ちます

7\. 2 つ目のサイトに対応するタブをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX9.jpg)

8\. *TargetSite-TestVM* という名前の仮想マシンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX10.jpg)

9\. この仮想マシンの IP アドレスが、ソースサイトの仮想マシン **Mission Critical Workload 1** が ping を行っている先の IP アドレスであることを確認します

10\. **Mission Critical Workload 1** タブをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX11.jpg)

    - ping コマンドの後に Enter を押下します
    - **Control-C** で ping を停止します
    - ping 172.16.4.2 とタイプします。IP アドレスはこの仮想マシンの IP アドレスになります

11\. このタブを閉じるために *X* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX12.jpg)

12\. 最初のタブをクリックし、オンプレミスの vCenter に戻ります

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX13.jpg)

13\. *Actions* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX14.jpg)

14\. *Hybridity Actions* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX15.jpg)

15\. *Migrate to the Cloud* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX16.jpg)

16\. *(Specify Destination Container)* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX17.jpg)

17\. *RedwoodCluster* を選択します
18\. *Select Destination* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX18.jpg)

19\. *(Select Storage)* ボタンをクリックします
20\. *cloudStorage* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX19.jpg)

21\. *(Select Virtual Disk Format)* をクリックします
22\. *Same format as source* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX20.jpg)

23\. *(Select Migration Type)* をクリックします
24\. *vMotion* を選択します
25\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX21.jpg)

26\. *Validation is Successful* メッセージを確認します
27\. *Finish* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX22.jpg)

28\. *Home*  ボタンをクリックします
29\. 左ペインにて *HCX* クリックします
30\. *Migration* タブをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX23.jpg)

31\. vMotion による移行の進捗を確認します
32\. *Refresh* タブをクリックし進捗をアップデートします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX24.jpg)

33\. vMotion が完了した後、2 つ目のタブでターゲット サイトの vCenter を開きます

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX25.jpg)

34\. *Mission Critical Workload 1* がターゲット サイトに無事移行されたことを確認できます。*Mission Critical Workload 1* をクリックします

35\. コンソール タブを開きます

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX26.jpg)

    - 前の手順で実行した ping がそのまま実行されていることを確認します。仮想マシンが vMotion による無停止での移行に成功しました。
    - *Control-C* を押し ping を停止させます

36\. ブラウザの *X* をクリックしコンソール タブを閉じます

<!--
### Reverse Migration

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX27.jpg)

37\. Click on the first tab in the browser

38\. Click the *Migrate Virtual Machines* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX28.jpg)

39\. Click the *Reverse Migration* checkbox

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX29.jpg)

40\. Click the checkbox for *Mission Critical Workload 1*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX30.jpg)

41\. Click on *(Specify Destination Container)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX31.jpg)

42\. Select *Tier 0 Workloads*

43\. Click *Select Destination* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX32.jpg)

44\. Click on *(Select Storage)*

45\. Select *onpremStorage*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX33.jpg)

46\. Click *(Select Virtual Disk Format)*

47\. Select *Same format as source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX34.jpg)

48\. Click on *(Select Migration Type)*

49\. Select *vMotion*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX35.jpg)

50\. Click *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX36.jpg)

51\. Once *Validation is Successful* message appears

52\. Click *Finish* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX37.jpg)

53\. Check the progress of the migration

54\. Click *Refresh* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX38.jpg)

55\. Click the *Home* button

56\. Click *Hosts and Clusters* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX39.jpg)

57\.. Click on the *Mission Critical Workload 1* VM, you can see that the Reverse migration to the on-premises vCenter was successful
-->

### 逆方向のマイグレーション

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX27.jpg)

37\. ブラウザの最初のタブをクリックします

38\. *Migrate Virtual Machines* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX28.jpg)

39\. *Reverse Migration* チェック ボックスをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX29.jpg)

40\. *Mission Critical Workload 1* のチェック ボックスをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX30.jpg)

41\. *(Specify Destination Container)* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX31.jpg)

42\. *Tier 0 Workloads* を選択します

43\. *Select Destination* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX32.jpg)

44\. *(Select Storage)* をクリックします

45\. *onpremStorage* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX33.jpg)

46\. *(Select Virtual Disk Format)* をクリックします

47\. *Same format as source* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX34.jpg)

48\. *(Select Migration Type)* をクリックします

49\. *vMotion* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX35.jpg)

50\. *Next* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX36.jpg)

51\. *Validation is Successful* メッセージが表示されます

52\. *Finish* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX37.jpg)

53\. マイグレーションの進捗を確認します

54\. *Refresh* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX38.jpg)

55\. *ホーム* ボタンをクリックします

56\. *ホストおよびクラスタ* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX39.jpg)

57\. *Mission Critical Workload 1* 仮想マシンをクリックし、オンプレミスの vCenter へ逆方向のマイグレーションが成功していることを確認します

<!--
## HCX - Bulk Migration

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk1.jpg)

1\. In your Chrome browser click the *HCX-Bulk* bookmark

2\. Click on the *X* to enlarge the main screen

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk2.jpg)

3\. As with the vMotion module, in this example we also have a source site (on-premises) vCenter

4\. Click on the second tab in the browser to view the Target site vCenter

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk3.jpg)

5\. Click on the tab for the on-premises vCenter

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk4.jpg)

6\. Click on the *Home* button

7\. Click *HCX*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk5.jpg)

8\. Click on the *Migration* tab

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk6.jpg)

9\. Click the *Migrate Virtual Machines* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk7.jpg)

10\. Select *Tier 1 Workloads* in the left pane

11\. Click the *checkbox* to select all VM's

12\. Click *(Specify Destination Container)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk8.jpg)

13\. Select *RedwoodCluster* as the Destination Container

14\. Click the *Select Destination* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk9.jpg)

15\. Click *(Select Storage)*

16\. Select *cloudStorage*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk10.jpg)

17\. Click *(Select Virtual Disk Format)*

18\. Select *Same format as source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk11.jpg)

19\. Click *(Select Migration Type)*

20\. Select *Bulk Migration*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk12.jpg)

21\. Click on *HelpDesk Workload 1* to see the options for this workload

22\. Click on *HelpDesk Workload 2* to see the options for this workload

23\. Click on the *sidebar* to view all the options

24\. Click the *Next* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk13.jpg)

25\. Wait for *Validation is Successful* message

26\. Click *Finish* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk14.jpg)

27\. Check Progress of the migrations

28\. Click *Refresh* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk15.jpg)

29\. Once migrations complete, click the *Hosts and Clusters* button

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk16.jpg)

30\. Click on the second tab to view the cloud vCenter

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk17.jpg)

31\. You can see that both workloads have successfully migrated to the Target vCenter
-->

## HCX - バルク マイグレーション

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk1.jpg)

1\. Chrome ブラウザで *HCX-Bulk* ブックマークをクリックします

2\. *-* をクリックしてメイン スクリーンを最大化します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk2.jpg)

3\. vMotion のモジュールのように、この例でもソース サイト (オンプレミス) の vCenter があります

4\. ブラウザの二つ目のタブをクリックし、ターゲット サイトの vCenter を確認します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk3.jpg)

5\. オンプレミスの vCenter のタブをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk4.jpg)

6\. *ホーム* ボタンをクリックします

7\. *HCX* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk5.jpg)

8\. *Migration* タブをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk6.jpg)

9\. *Migrate Virtual Machines* button ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk7.jpg)

10\. 左ペインの *Tier 1 Workloads* を選択します

11\. 全ての仮想マシンを選択するために *checkbox* をクリックします

12\. *(Specify Destination Container)* をクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk8.jpg)

13\. 行き先のコンテナとして *RedwoodCluster* を選択します

14\. *Select Destination* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk9.jpg)

15\. *(Select Storage)* をクリックします

16\. *cloudStorage* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk10.jpg)

17\. *(Select Virtual Disk Format)* をクリックします

18\. *Same format as source* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk11.jpg)

19\. *(Select Migration Type)* をクリックします

20\. *Bulk Migration* を選択します

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk12.jpg)

21\. ワークロードのオプションを確認するために *HelpDesk Workload 1* をクリックします

22\. ワークロードのオプションを確認するために *HelpDesk Workload 2* をクリックします

23\. 全てのオプションを確認するために *sidebar* をクリックします

24\. *Next* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk13.jpg)

25\. *Validation is Successful* メッセージが表示されるのを待機します

26\. *Finish* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk14.jpg)

27\. マイグレーションの進捗を確認します

28\. *Refresh* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk15.jpg)

29\. マイグレーションが完了したら、*ホストおよびクラスタ* ボタンをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk16.jpg)

30\. クラウド側の vCenter を確認するために、2つ目のタブをクリックします

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/Bulk17.jpg)

31\. 両方のワークロードがターゲットの vCenter への移行が成功したことを確認します