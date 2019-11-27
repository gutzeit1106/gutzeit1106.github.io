# Microsoft Ignite 2019 で発表された新機能のまとめ(Storage編)


Microsoft Ignite 2019 の Storage周りの update まとめです。
公開情報がまだ十分にない preview な機能の紹介が多かった印象でした。

## 1. Azure Disk
ディスク関連の update は、 Azure VM とかぶっているのが多くあります。

### SAP HANA Certification on Ultra Disk and M/E series VMs
8月に GA された Ultra Disk と　M or E シリーズ VM の構成が、SAP HANA の認定を受けた。

参照:[Azure Ultra Disk Storage の一般公開に関するお知らせ](https://azure.microsoft.com/ja-jp/blog/announcing-the-general-availability-of-azure-ultra-disk-storage/)

### Shared Disk Support for Ultra and Premium SSD
複数の仮想マシンからディスクを共有する Azure Shared Disks のプレビューが開始されたそうです。<br>
プレビュー申し込みは次の Forms から。http://aka.ms/SharedDiskSignUp

### Direct uplocad to Managed Disk
オンプレから VHD を持ち込む際に以前まで一度、 Azure Storage にファイルをアップロードしてから、Managed Disk に変換する必要がありましたが、直接 Managed Disk にアップロードできるようになるプレビュー機能が紹介されていました。

参照:[Introducing the preview of direct-upload to Azure managed disks](https://azure.microsoft.com/en-au/blog/introducing-the-preview-of-direct-upload-to-azure-managed-disks/)

###  Better performance with bursting enhancement and smaller size offerings on Azure Disks
Premium SSD と Standard SSD で、4, 8, 16GB のオプションが追加されました。
また、Premium SSD で P20 (512　GB 以下)のディスクは、IOPS とスループット性能がバーストし、一時的に最大で30倍の性能(3500IOPS, 170MiB/s)まで引き上げることが可能なプレビュー機能が発表されました。

参照:[Premium SSD bursting](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-bursting)

### Enabling server-side encryption with customer-managed keys for Azure Disks
Managed Disks のサーバーサイド暗号化（SSE）をKeyvault上のユーザ管理のkeyで実施できるようになるプレビュー機能が発表されました。

参照:[Preview: Server-side encryption with customer-managed keys for Azure Managed Disks](https://azure.microsoft.com/ja-jp/blog/preview-server-side-encryption-with-customer-managed-keys-for-azure-managed-disks/)

### Optimize data protection with incremental snapshots and direct upload on Azure Disks
Managed Diskのスナップショットは、今まで差分コピーでなく取得時点のフルコピーでしたが、差分スナップショットがプレビュー機能で発表されました。

参照:[Introducing cost-effective increment snapshots of Azure managed disks in preview](https://azure.microsoft.com/en-us/blog/introducing-cost-effective-increment-snapshots-of-azure-managed-disks-in-preview/)


## 2. Azure Files
### Azure Backup integration
セッションだと　very very close to announce と言っていたので、まだ一応 GA ではないようですが、もうすぐ Azure Backup と　Azure Files の統合機能が GA される予定のようです。

参照:[Back up and restore Azure file shares](https://docs.microsoft.com/en-us/azure/backup/backup-azure-files)

### Support for NFS v4.1 (Preview)
従来のSMB 2.1, 3.0, REST　でのアクセスに加えて、NFS v4.1　でのアクセスがサポートされる機能が開発中とのこと。

### Active Directory Authentication (Preview)
2019年8月に [Azure AD DSとAzure Filesの認証統合のGAがアナウンス](https://azure.microsoft.com/en-us/blog/better-security-with-enhanced-access-control-experience-in-azure-files/)されましたが、ユーザ管理の AD にジョインして認証統合させる機能が Limited Preview で開発されているみたいです。<br>
Survey Form　は次にあります。
http://aka.ms/azurefilesADAuthPreviewSurvey

### Cost-effective tiers for secoundary data (Preview)
Azure Files にも IO 頻度などに応じてコスト最適化を図れる Cool, Hot Tier機能を設ける予定があるようです。

### Private Endpoint (Preview)
Azure Storage に仮想ネットワークのIP空間に private ipを割り当てる Private Endpointt が紹介されていました。

参照:
[Azure Files networking considerations](http://aka.ms/AzureFilesNetwork)
[Configure a Point-to-Site (P2S) VPN on Windows for use with Azure Files](https://docs.microsoft.com/ja-jp/azure/storage/files/storage-files-configure-p2s-vpn-windows#create-a-private-endpoint-preview)

## 3. Azure Blob
### Multi protocol Access with Blob and ADLS Gen2 API
Azure Data Lake Storage Gen2 上へのデータアクセスが、BLOB　と　ADLS のどちらのエンドポイントからもアクセスできる機能が、 GA らしいです。

### Custome controlled failover 
GRSのマニュアルフェールオーバー機能は2019年中にGAの見込みだとか。
プレビュー公開時に、「[Public PreviewになったAzureストレージのマニュアルフェイルオーバーを早速試して見た](https://qiita.com/yyoneda1106/items/80387725767f4794b7bd)」で試したやつですね。

### Reserved Capacity with cost saving (GA)
Storage 容量を1年ないしは3年予約することでストレージコストの割引を受けることができる Storage Reserved Capacity が GA されたようです。
料金ページの FAQ にも説明があります。

[Azure Storage Reserved Capacity FAQs](https://azure.microsoft.com/ja-jp/pricing/details/storage/data-lake/)

英語の予約系のコストオプションのページも英語ドキュメントは更新されているみたいです。

[What are Azure Reservations?](https://docs.microsoft.com/en-us/azure/billing/billing-save-compute-costs-reservations)

### NFS v3 support on Blob Storage
NFS v3　経由で Blob Storage　経由にアクセスできる機能が近くプレビュー公開される。

## 4. Others
### Azure HPC Cache generally available
VMから Azure Blob や Azure Netapp Files の間に配置して、Cacheする Azure HPC Cache　が GA されたそうです。

参照:[Azure HPC Cache is now available](https://azure.microsoft.com/ja-jp/updates/azure-hpc-cache-is-now-available/)