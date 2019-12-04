# Microsoft Ignite 2019 で発表された新機能のまとめ(VM/VMSS編)


今週は Microsoft Ignite 2019 で盛り上がった一週間でした。  
自分の周りでは、Keynote で取り上げられていた、Azure Arc や Azure Synapse が特に話題になっていた気がします。
それ以外にも、新機能の発表、プレビュー機能の GA ニュースが山盛りでした。
発表された項目は、[Book of News Microsoft Ignite 2019](https://news.microsoft.com/wp-content/uploads/prod/sites/563/2019/11/Ignite-2019-Book-of-News.pdf)に記載がありますが、index だけで7ページもあります。
しばらくキャッチアップに注力せねば...  
そんな中、Azure の IaaS 系のサービスについては最新状況をだいたい把握しているつもりでしたが、VM Scaleset(VMSS) で初めて認識した機能があったので、その機能の検証と、ついでに Ignite 2019 で触れていた VM/VMSS の機能についてまとめてみました。

### 1. Azure Da_v4 and Das_v4 virtual machines 
新しい VM サイズが GA されました。AMD EPYC 7452 プロセッサを搭載した Da_v4 と Das_v4 になります。

### 2. Serial Console for Azure Government Cloud public preview
Azure Government Cloud でシリアルコンソール機能がパブリックプレビューになりました。通常のパブリックな Azure だととっくに GA されているので、これは日本のユーザにはあんまり関係なさそうな気がします。

### 3. Azure Generation 2 virtual machines generally available
Azure 上で hyper-V 第2世代の仮想マシンが利用できる機能が GA になりました。
第2世代の仮想マシンでは、OS の boot が UEFI 形式になるため、OS ディスクを 2TB 以上にすることができます。（第一世代はレガシー BIOS だったので、OS ディスクは 2TB 以下しか使用できませんでした。データディスクは 2TB 以上使えます。）
ただし、Azure の機能で Bitlocker などを有効化する Azure Disk Encryption や, DR 対策でよく使用される Azure Site Recovery は、まだ第2世代仮想マシンには対応していないようなので今後の対応を待つ必要がありそうです。

参考:
[Support for generation 2 VMs on Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/generation-2)

### 4. New features for Azure virtual machine scale sets
仮想マシンスケールセットでいくつかプレビューの機能が紹介されました。
ドキュメントは以下の通りです。実用的な機能が増えてきた印象です。1の Orchestration mode は知らなかった機能なので[別途調べて](../../../..//2019/11/06/20191106_vmss_orchestrationmode/)みました。

1. [Orchestration mode (preview)](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/orchestration-modes)

2. [Preview: Use custom scale-in policies with Azure virtual machine scale sets](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-scale-in-policy)

3. [Shared Image Galleries overview](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/shared-image-galleries)

4. [Terminate notification for Azure virtual machine scale set instances (Preview)](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-terminate-notification)

5. [Instance Protection for Azure virtual machine scale set instances (Preview)](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-instance-protection)

2の scale-in policies は、スケールインする際に削除されるインスタンスを選ぶ戦略を選択できるオプションです。
3はユーザが作ったイメージをいろいろと共有できる Shared Image Galleries と VMSS の統合です。
4 Terminate notification は、スケールインの際にインスタンス削除開始までの時間を制御できるオプションです。終了処理をしてからスケールインしたい時などに有用かと思います。
5の Instance Protection は、特定のインスタンスを自動スケールなどの操作から保護できるオプションです。  

### 5. Azure HBv2 virtual machines coming soon
AMD EPYCが搭載されたHPC向け（RDMA使用可）の仮想マシンサイズ HBv2 の紹介です。このサイズはまだGAではないみたいです。

参考:[ハイ パフォーマンス コンピューティング用の新しい HBv2 Azure 仮想マシンの概要](https://azure.microsoft.com/ja-jp/blog/introducing-the-new-hbv2-azure-virtual-machines-for-high-performance-computing/)

###  6. Azure Ea v4 and Eas v4 series virtual machines
AMD EPYC 7452 が搭載された仮想マシンサイズ Ea v4/Eas v4 が GA されました。

###  7. Azure NVv4 virtual machine public preview
AMD EPYC 7002 プロセッサと Radeon MI25 GPU が搭載された NVv4 がパブリックプレビューになりました。

参考:[Introducing NVv4 Azure Virtual Machines for GPU visualization workloads](https://azure.microsoft.com/en-us/blog/introducing-nvv4-azure-virtual-machines-for-gpu-visualization-workloads/)

### 8. Azure NDv2 virtual machines public preview
NVIDIA Tesla V100のGPUとIntel Xeon Platinum 8168 が搭載された NDv2 がパブリックプレビューになりました。

### 9. Proximity placement groups now generally available
Proximity placement groups(日本語だと近接通信配置グループ)が GA されたようです。PPGと略しているみたい。
PPG は、可用性ゾーンよりもさらにデータセンター内の物理的な距離を近づけて仮想マシンを配置できるオプションです。物理的距離が近いので、仮想マシン間の通信レイテンシが小さくなることが期待されます。

### 10. Azure Spot Virtual Machines
ローコストなAzure Spot VM が VM/VMSS で 2020 年の始めくらいにプレビュー利用できるようになるらしい。ドキュメントが見当たらなかったので今後出てくると予想。

### 11. Better performance with bursting enhancement and smaller size offerings on Azure Disks
Premium SSD と Standard SSD で、4, 8, 16GB のオプションが追加されました。
また、Premium SSD で P20 (512　GB 以下)のディスクは、IOPS とスループット性能がバーストし、一時的に最大で30倍の性能(3500IOPS, 170MiB/s)まで引き上げることが可能なプレビュー機能が発表されました。

参考:[Premium SSD bursting](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-bursting)

### 12. Enabling server-side encryption with customer-managed keys for Azure Disks
Managed Disks のサーバーサイド暗号化（SSE）をKeyvault上のユーザ管理のkeyで実施できるようになるプレビュー機能が発表されました。

参考:[Preview: Server-side encryption with customer-managed keys for Azure Managed Disks](https://azure.microsoft.com/ja-jp/blog/preview-server-side-encryption-with-customer-managed-keys-for-azure-managed-disks/)


### 13. Optimize data protection with incremental snapshots and direct upload on Azure Disks
Managed Diskのスナップショットは、今まで差分コピーでなく取得時点のフルコピーでしたが、差分スナップショットがプレビュー機能で発表されました。

参考:[Introducing cost-effective increment snapshots of Azure managed disks in preview](https://azure.microsoft.com/en-us/blog/introducing-cost-effective-increment-snapshots-of-azure-managed-disks-in-preview/)

### 14. Azure Shared Image Gallery now supports specialized images in preview
Azure Shared Image Galleryで特殊化イメージをサポートする機能がプレビューとなりました。

参考:[Azure Shared Image Gallery now supports specialized images in preview](https://azure.microsoft.com/ja-jp/updates/shared-image-gallery-specialized-preview/)