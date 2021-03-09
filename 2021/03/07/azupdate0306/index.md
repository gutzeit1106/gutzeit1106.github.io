# Azure Update - 2021.3.06


Ignite があったせいかいつにもまして update が多い。
人力での更新は時間がかかりすぎるので、Logic App, Functions 組み合わせて省力化を進めてるが、もっと効率化していこう。

## IaaS & Netowrking
### VM
[Improve Azure Spot Virtual Machines runtime and simulate evictions with new features in public preview](https://azure.microsoft.com/en-us/updates/improve-azure-spot-virtual-machines-runtime-and-simulate-evictions-with-new-features-in-public-preview/) on 2021-03-03<br>
容量が原因で削除された VMSS の Spot VM を AI によって自動的に復元して、ターゲットのインスタンス数を確保しようとする機能らしい。
コストとインスタンス数確保のいいとこどり的な機能。Public Preview。   
詳細は[ここ](https://docs.microsoft.com/ja-jp/azure/virtual-machine-scale-sets/use-spot#try--restore-preview)

[On-demand capacity reservations in public preview](https://azure.microsoft.com/en-us/updates/ondemand-capacity-reservations-in-public-preview/) on 2021-03-03<br>
仮想マシンの容量を先取り確保する機能が public preview。
別途申し込みが必要なようだけども allocation failure 対策に良いかもしれない。

[General availability: New Simplified NSX networking experience for Azure VMware Solution](https://azure.microsoft.com/en-us/updates/general-availability-new-simplified-nsx-networking-experience-for-azure-vmware-solution/) on 2021-03-04<br>

[Zone Redundant Storage (ZRS) option for Azure managed disks in limited preview](https://azure.microsoft.com/en-us/updates/zone-redundant-storage-zrs-option-for-azure-managed-disks-in-limited-preview/) on 2021-03-02<br>
Managed Disk(Premium SSD / Standard SSD) でゾーン冗長オプションが Limited Preview に。

[Public preview: Change performance tiers for Premium SSD managed disks with no downtime](https://azure.microsoft.com/en-us/updates/public-preview-change-performance-tiers-for-premium-ssd-managed-disks-with-no-downtime/) on 2021-03-02<br>
Premium SSD で実行中の VM に接続されていてもダウンタイムなしに パフォーマンス Tier を変更できる機能が Public Preview。これどうやってるんだろうか。

[On-demand disk bursting for Premium SSDs now in public preview ](https://azure.microsoft.com/en-us/updates/ondemand-disk-bursting-for-premium-ssds-now-in-public-preview/) on  2021-03-02<br>

[Azure trusted launch for Virtual Machines now in public preview](https://azure.microsoft.com/en-us/updates/trustedlaunch/) on 2021-03-03<br>
gen2 VM で vTPM, セキュアブートが public preview

[Windows Server 2022 is now available in preview.](https://azure.microsoft.com/en-us/updates/windows-server-2022-is-now-available-in-preview/) on 2021-03-02<br>
次は 2022 なんですね。Azure 連携と Security が強化されそうな予感。

[New orchestration mode for Azure Virtual Machine Scale Sets now in public preview](https://azure.microsoft.com/en-us/updates/new-orchestration-mode-for-azure-virtual-machine-scale-sets-now-in-public-preview/) on  2021-03-03<br>
Flexible orchestration mode が追加されて、通常 VM を VMSS 配下で管理出来たり。
[既存のモードとの比較](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-orchestration-modes#a-comparison-of-flexible-uniform-and-availability-sets)

[Automatic VM guest patching is now in public preview for Linux VMs](https://azure.microsoft.com/en-us/updates/automatic-vm-guest-patching-now-in-preview-linux/) on 2021-03-03<br>
Linux VM でゲスト OS の自動パッチ適用機能が public previewに・

[Azure VMware Solution now generally available in the Southeast Asia region](https://azure.microsoft.com/en-us/updates/azure-vmware-solution-now-generally-available-in-the-southeast-asia-region/) on 2021-03-05<br>

### AKS
[Public preview: Azure Key Vault CSI driver support in Azure Kubernetes Service](https://azure.microsoft.com/en-us/updates/public-preview-azure-key-vault-csi-driver-support-in-azure-kubernetes-service/) on  2021-03-02<br>

[Confidential computing nodes (DCSv2) on Azure Kubernetes Service (AKS) is generally available](https://azure.microsoft.com/en-us/updates/confidential-computing-nodes-aks-ga/) on 2021-03-02<br>

[Azure Arc enabled Kubernetes is now generally available](https://azure.microsoft.com/en-us/updates/azure-arc-enabled-kubernetes-is-now-generally-available/) on 2021-03-02 <br>

[General availability: Just-In-Time Access support in AKS ](https://azure.microsoft.com/en-us/updates/general-availability-justintime-access-support-in-aks/) on  2021-03-02<br>

[General availability: Deploy WebLogic on Azure Kubernetes Service (AKS) using custom Docker images](https://azure.microsoft.com/en-us/updates/general-availability-deploy-weblogic-on-azure-kubernetes-service-aks-using-custom-docker-images/) on 2021-03-02<br>

[Public preview: Dynamic IP allocation & enhanced subnet support in AKS ](https://azure.microsoft.com/en-us/updates/public-preview-dynamic-ip-allocation-enhanced-subnet-support-in-aks/) on 2021-03-02 <br>

[General availability: App Gateway ingress controller add-on for AKS](https://azure.microsoft.com/en-us/updates/general-availability-app-gateway-ingress-controller-addon-for-aks/) on  2021-03-02<br>

### VPN
[Multiple new features for Azure VPN Gateway in public preview](https://azure.microsoft.com/en-us/updates/multiple-new-features-for-azure-vpn-gateway-in-public-preview/) on  2021-03-02 17:00:24Z<br>

### Load Balancer
[Azure Load Balancer support for IP-based backend pool management is now generally available](https://azure.microsoft.com/en-us/updates/iplbga/) on 2021-03-02<br>
Standard LB で NIC でなく IP ベースでバックエンドに配置可能に


## Management
### Container insights
[General availability of Persistent Volume monitoring & Reports tab in Container Insights](https://azure.microsoft.com/en-us/updates/general-availability-of-persistent-volume-monitoring-reports-tab-in-container-insights/) on 2021-03-02<br>
AKS の Persistent Volume の監視機能が追加

[General availability: Azure monitor for containers now supports Pods & Replica set live logs in AKS resource view](https://azure.microsoft.com/en-us/updates/azmon-livelogs-pods/) on 2021-03-05<br>
AKS の Pods と Replica set live logs も監視可能に

### Application insights
[General availability: Application insights no-code enablement on Node.js Linux App Service Environments](https://azure.microsoft.com/en-us/updates/general-availability-application-insights-nocode-enablement-on-nodejs-linux-app-service-environments/) on 2021-03-02<br>
Linux Web App の Node.js でノーコードで Application Inisightsが有効にできるようになった

### Azure Backup
[Azure Backup for SAP HANA: Incremental backup is now generally available](https://azure.microsoft.com/en-us/updates/azure-backup-for-sap-hana-incremental-backup-is-now-generally-available/) on  2021-03-01<br>
SAP HANA での増分バックアップが GA

[Azure Backup: Operational backup for Azure Blobs is now in public preview](https://azure.microsoft.com/en-us/updates/azure-backup-blob-op-backup-preview/) on  2021-03-01<br>

[Limited preview: Azure Backup now supports archive tier for backup of Azure Virtual Machines and SQL Server in Azure VMs ](https://azure.microsoft.com/en-us/updates/azure-backup-now-supports-archive-tier-for-backup-of-azure-virtual-machines-and-sql-server-in-azure-vms/) on  2021-03-02 17:00:10Z<br>


[Backup Reports is now generally available](https://azure.microsoft.com/en-us/updates/backup-reports-is-now-generally-available/) on  2021-03-02 17:00:03Z<br>

### Azure Automate
[Public preview: Announcing new capabilities for Azure Automanage](https://azure.microsoft.com/en-us/updates/public-preview-announcing-new-capabilities-for-azure-automanage/) on  2021-03-02 17:00:11Z<br>

### Azure Security Center
[Azure Security Center: Public preview updates for February 2021](https://azure.microsoft.com/en-us/updates/asc-february2021-2/) on 2021-03-01<br>

[Azure Security Center: General availability updates for February 2021](https://azure.microsoft.com/en-us/updates/asc-february2021-1/) on 2021-03-01<br>

### Azure Migrate
[Generally available: Plan your migration to Azure VMware Solution using Azure Migrate](https://azure.microsoft.com/en-us/updates/azure-migrate-azure-vmware-solution-assessment-ga/) on 2021-03-02<br>
Azure Migrate で Azure VMware Solution の移行評価が対応

[Public preview: Containerize and migrate apps to Azure Kubernetes Service with Azure Migrate: App Containerization ](https://azure.microsoft.com/en-us/updates/public-preview-containerize-and-migrate-apps-to-azure-kubernetes-service-with-azure-migrate-app-containerization/) on 2021-03-02<br>
Azure Migrate で AKS 移行をサポートする App Containerization 機能が追加

PaaS
---
### APIM
[General availability: Azure API Management now has named values integration with Azure Key Vault](https://azure.microsoft.com/en-us/updates/general-availability-azure-api-management-now-has-named-values-integration-with-azure-key-vault/) on  2021-03-03<br>

[Azure API Management extension for Visual Studio Code now generally available](https://azure.microsoft.com/en-us/updates/azure-api-management-extension-for-visual-studio-code-now-generally-available/) on  2021-03-03<br>

### Redis
[New Enterprise and Enterprise Flash tiers for Azure Cache for Redis now generally available](https://azure.microsoft.com/en-us/updates/new-enterprise-and-enterprise-flash-tiers-for-azure-cache-for-redis-now-generally-available/) on  2021-03-02<br>
Azure Cache for Redis で Redis Enterprise Tier が一般提供開始。

### Event Hub
[Event Hubs on Azure Stack Hub for disconnected scenarios is now generally available](https://azure.microsoft.com/en-us/updates/event-hubs-on-azure-stack-hub-for-disconnected-scenarios-is-now-generally-available/) on  2021-03-01 17:00:04Z<br>

### Azure Communication Services
[Public preview: Azure Communication Services interoperability with Microsoft Teams ](https://azure.microsoft.com/en-us/updates/public-preview-azure-communication-services-interoperability-with-microsoft-teams/) on  2021-03-02 17:00:13Z<br>

### Spring Cloud
[Azure Spring Cloud: General availability of Managed Virtual Network and Autoscale](https://azure.microsoft.com/en-us/updates/azure-spring-cloud-general-availability-of-managed-virtual-network-and-autoscale/) on  2021-03-02 17:00:05Z<br>

## SQL
### Azure SQL DB
[Public preview: Maintenance window for Azure SQL Database and Azure SQL Managed Instance](https://azure.microsoft.com/en-us/updates/public-preview-maintenance-window-for-azure-sql-database-and-azure-sql-managed-instance/) on  2021-03-02<br>
SQL Database と SQL Managed Instance でメンテナンスウィンドウ機能が Public Preview

[Advance notifications for Azure SQL Database now in public preview](https://azure.microsoft.com/en-us/updates/advance-notifications-for-azure-sql-database-now-in-public-preview/) on 2021-03-02<br>
計画メンテナンスの事前通知機能が Public Preview


[Public preview: At scale discovery and assessment for SQL Server migration to Azure SQL](https://azure.microsoft.com/en-us/updates/public-preview-at-scale-discovery-and-assessment-for-sql-server-migration-to-azure-sql/) on  2021-03-02 17:00:23Z<br>


### Cosmos
[Azure Cosmos DB API for MongoDB supports version 4.0](https://azure.microsoft.com/en-us/updates/azure-cosmos-db-api-for-mongodb-supports-version-40/) on  2021-03-02 17:00:18Z<br>


## Big Data & AI

[Azure Machine Learning - Arc enabled machine learning](https://azure.microsoft.com/en-us/updates/azure-machine-learning-arc-enabled-machine-learning/) on  2021-03-02 17:00:14Z<br>

###  Azure Purview
[Public preview: Azure Purview powered search in Azure Synapse Analytics workspaces](https://azure.microsoft.com/en-us/updates/public-preview-azure-purview-powered-search-in-azure-synapse-analytics-workspaces/) on  2021-03-02 17:00:19Z<br>

[Public preview: Scan and classify Amazon AWS S3 data using Azure Purview](https://azure.microsoft.com/en-us/updates/public-preview-scan-and-classify-amazon-aws-s3-data-using-azure-purview/) on  2021-03-02 17:00:20Z<br>

[Scan and view lineage of data stored in SAP ERP and Oracle DB using Azure Purview](https://azure.microsoft.com/en-us/updates/scan-and-view-lineage-of-data-stored-in-sap-erp-and-oracle-db-using-azure-purview/) on  2021-03-02 17:00:19Z<br>

[Public preview: Azure Purview Data Catalog now supports hierarchical glossary](https://azure.microsoft.com/en-us/updates/public-preview-azure-purview-data-catalog-now-supports-hierarchical-glossary/) on  2021-03-02 17:00:20Z<br>

[Public preview: Azure Purview Bulk Edit enhancements](https://azure.microsoft.com/en-us/updates/public-preview-azure-purview-bulk-edit-enhancements/) on  2021-03-04 20:48:28Z<br>

[Public preview: Azure Purview Resource Set configuration enhancements](https://azure.microsoft.com/en-us/updates/public-preview-azure-purview-resource-set-configuration-enhancements/) on  2021-03-03 17:00:18Z<br>

[Public preview: Register Azure subscriptions and Resource Groups with Azure Purview ](https://azure.microsoft.com/en-us/updates/public-preview-register-azure-subscriptions-and-resource-groups-with-azure-purview/) on  2021-03-03 17:00:17Z<br>

[Azure Managed Instance for Apache Cassandra service now in public preview](https://azure.microsoft.com/en-us/updates/azure-managed-instance-for-apache-cassandra-service-now-in-public-preview/) on  2021-03-02 17:00:13Z<br>

[Azure Machine Learning - Generally available announcements at Ignite, March 2021. ](https://azure.microsoft.com/en-us/updates/azure-machine-learning-generally-available-announcements-at-ignite-march-2021/) on  2021-03-02 17:00:14Z<br>

[Azure Stream Analytics Dedicated now generally available](https://azure.microsoft.com/en-us/updates/azure-stream-analytics-dedicated-now-generally-available/) on  2021-03-02 17:00:21Z<br>

## IoT
[Device Update for IoT Hub in public preview](https://azure.microsoft.com/en-us/updates/device-update-for-iot-hub-in-public-preview/) on 2021-03-02 <br>

[Azure Sphere OS version 21.02 Update 1 is now available for evaluation](https://azure.microsoft.com/en-us/updates/azure-sphere-os-version-2102-update-1-is-now-available-for-evaluation/) on  2021-03-01<br>

[Azure Sphere version 21.02 is now available ](https://azure.microsoft.com/en-us/updates/azure-sphere-version-2102-is-now-available/) on 2021-03-04 <br>



## Others
[Azure Remote Rendering is now generally available ](https://azure.microsoft.com/en-us/updates/azureremoterenderingga/) on  2021-03-02 17:00:25Z<br>

[Azure Object Anchors is now in public preview](https://azure.microsoft.com/en-us/updates/azureobjectanchorspreview/) on  2021-03-02 17:00:24Z<br>

[General availability: Announcing private Azure Marketplace](https://azure.microsoft.com/en-us/updates/general-availability-announcing-private-azure-marketplace/) on  2021-03-02 16:00:02Z<br>

[Microsoft Power Fx: The open-source low-code programming language is in public preview](https://azure.microsoft.com/en-us/updates/microsoft-power-fx-the-opensource-lowcode-programming-language-is-in-public-preview/) on  2021-03-02 17:00:25Z<br>
