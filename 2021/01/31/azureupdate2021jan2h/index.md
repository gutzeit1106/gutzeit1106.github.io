# Azure Update - 2021.Jan 2H


月に 2 回くらい更新をまとめるかと思っていたけど半月分だけでもなかなかの量だなぁ。<br>
リージョンの追加とかの更新は省いた。

###  Azure AD
[99.99% uptime for Azure Active Directory Premium customers is coming April 1st, 2021](https://azure.microsoft.com/ja-jp/updates/9999-uptime-for-azure-active-directory-premium-customers-is-coming-april-1st-2021/)<br>
Azure AD Premium の SLA が 4/1 から従来の 99.9% から 99.99％ に引き上げられる。

### SAP
[KSAP and Microsoft expand partnership and integrate Microsoft Teams across solutions](https://news.microsoft.com/2021/01/22/sap-and-microsoft-expand-partnership-and-integrate-microsoft-teams-across-solutions/)<br>
Microsoft Teams と SAP を統合していくという話と、Azure 上での SAP HANA 利用の拡大のために戦略的パートナーシップを拡張したというアナウンス。

### Storage
[Generally available: Copy Blob support over private endpoints in Azure Storage](https://azure.microsoft.com/ja-jp/updates/copy-blob-support-over-private-endpoints/)<br>
プライベートエンドポイントを有効化していててもストレージ間コピーが可能になった。

[Public preview: Prevent Shared Key authorization on Azure Storage accounts](https://azure.microsoft.com/ja-jp/updates/prevent-shared-key-authorization-on-azure-storage-accounts/)<br>
アクセスキーとアクセスキーで署名されたSASを無効化できる機能がプレビュー

[Resource instance rules for access to Azure Storage now in public preview](https://azure.microsoft.com/en-us/updates/storage-resource-instance-rules-preview/)<br>
Managed Id を指定して特定のリソースインスタンスから Azure Storage への通信を許可する機能がプレビューのようです。
[ここ](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security#trusted-microsoft-services)にちょっと説明が乗っているけど、設定は下からできるみたい。

### Backup
[Azure Backup: Encryption at rest using customer-managed keys is now generally available](https://azure.microsoft.com/ja-jp/updates/azure-backup-cmk-encryption/)<br>
 Key Vault に保存されたユーザ管理のキーを使用して Recovery Service Vault に保存されたデータ暗号化できるようになった。

[Backup for Azure Managed Disk is in limited preview](https://azure.microsoft.com/ja-jp/updates/azure-disk-backup/)<br>
Managed Disk でバックアップを取れる

###  Site Recovery
[Azure Site Recovery update rollup 53 is now available - January 2021](https://azure.microsoft.com/ja-jp/updates/site-recovery-update-rollup-53-january-2021/)

### Monitor 
[Log Analytics Multi Scope Pin to Dashboards](https://techcommunity.microsoft.com/t5/azure-monitor/log-analytics-multi-scope-pin-to-dashboards/ba-p/2052228)<br>
Log Analytics のピン留め機能が便利になった。

[Azure Monitor Network Insights is now generally available](https://azure.microsoft.com/ja-jp/updates/azure-monitor-network-insights-is-now-generally-available/)<br>
ネットワーク系の監視エクスペリエンスが充実してきたみたい。[Azure Monitor for Networks](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/network-insights-overview)

[Changes in Azure Monitor Logs for the AzureDiagnostics table is now available](https://azure.microsoft.com/ja-jp/updates/changes-in-azure-monitor-logs-for-the-azurediagnostics-table-is-now-available/)<br>
AzureDiagnostics のテーブル構成が変更になった

[Public preview of Linux Diagnostics Agent 4.0](https://azure.microsoft.com/ja-jp/updates/public-preview-of-linux-diagnostics-agent-40/)<br>
LAD 4.0 がプレビュー。

[Automatic tracking of click events with Application Insights is now available](https://azure.microsoft.com/ja-jp/updates/automatic-tracking-of-click-events-with-application-insights-is-now-available/)<br>
JavaScript のクリックイベントを監視できるようになった

[Azure Monitor ITSM Connector for ServiceNow ITOM with Secure Export](https://azure.microsoft.com/ja-jp/updates/azure-monitor-itsm-connector-for-servicenow-itom-with-secure-export/)

###  Logic App
[Azure Logic Apps Running Anywhere - Built-In Service Bus Trigger: batching and session handling](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-service-bus-trigger/ba-p/2079995)<br>
Update ではないが Logic App で　Service Bus トリガー使う場合の blog 記事

[Azure Logic Apps - Authenticate with managed identity for Azure AD OAuth-based connectors](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-authenticate-with-managed-identity-for-azure-ad/ba-p/2066254)<br>
Update ではないが Logic App で MSI を使う場合のブログ記事

### App Service / Funtions
[Python 3.9 in Azure Functions is now in public preview](https://azure.microsoft.com/ja-jp/updates/python-39-in-azure-functions-is-now-in-public-preview/) <br>
python 3.9 が public previewに

[App Service Authentication portal experience is now in public preview](https://azure.microsoft.com/ja-jp/updates/app-service-authentication-portal-experience-is-now-in-public-preview/)<br>
App Service のポータルの認証設定画面が改善された。

[New App Service + Database create blade (preview)](https://azure.github.io/AppService/2021/01/26/webapp-db-create.html)<br>
App Service と SQL (Postgres or SQL Server) を同時に作成して、接続文字列をAppSettingに自動追加できるポータルブレードが追加

[New App Service Anti-virus Logs in Public Preview](https://azure.github.io/AppService/2020/12/09/AzMon-AppServiceAntivirusScanAuditLogs.html)<br>
Storage account, Log Analytics, Event Hub にログを転送できるウイルス対策スキャンが追加。

### APIM
[Azure API Management Updates - January, 2021](https://azure.microsoft.com/ja-jp/updates/azure-api-management-updates-january-2021/)<br>
cache-store ポリシーで cache-response で http レスポンスのキャッシュの細かな指定ができるようになったり、証明書を保存する key vault のアクティビティ検索などが拡充されたり。

### Sevice Fabric
[Azure Service Fabric 7.1 Tenth Refresh Release](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-1-tenth-refresh-release/ba-p/2096558) <br>
[Azure Service Fabric 7.2 Fifth Refresh Release](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-2-fifth-refresh-release/ba-p/2096575) <br>
.NET 5 apps for Windows on Service Fabric がプレビュー。.NET 5 apps for Linux on Service Fabric はService Fabric 8.0 で追加される予定。

[Azure Service Fabric Application upgrade time](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-service-fabric-application-upgrade-time/ba-p/2053047) <br>
アプリケーションのアップグレード時の時間がかかるポイントなどの解説

[How to rebuild new cluster by retaining rest of the resources in the Resource Group](https://techcommunity.microsoft.com/t5/azure-paas-blog/how-to-rebuild-new-cluster-by-retaining-rest-of-the-resources-in/ba-p/2069751)   
サポートされていない操作が原因でクラスターの状態が失われた場合に、リソースグループ内の他のリソース（ロードバランサー、パブリックIP、Vnetなど）を保持してSFクラスターを再作成/再構築する手順

[Azure Service Fabric Mesh Preview Retirement](https://azure.microsoft.com/ja-jp/updates/azure-service-fabric-mesh-preview-retirement/)   
既存デプロイは、2021年4月28日まで利用可能、新規デプロイは停止。
github の issue をみるに 2020年10月 くらいから新規デプロイに制限がかかってそう。<br>
https://github.com/Azure/service-fabric-mesh-preview/issues/392



### Cloud Service
[New Azure Cloud Services deployment model in public preview](https://azure.microsoft.com/ja-jp/updates/new-azure-cloud-services-deployment-model-in-public-preview/)<br>
Classic Cloud Service (PaaS) の ARM 後継サービスがプレビューに。

### AKS
[General availability: Optional Uptime SLA for Azure Kubernetes Service private clusters](https://azure.microsoft.com/ja-jp/updates/general-availability-optional-uptime-sla-for-azure-kubernetes-service-private-clusters/)<br>
AKSプライベートクラスターの Uptime SLA が GA

[Public preview: Azure Key Vault CSI driver support in Azure Kubernetes Service](https://azure.microsoft.com/ja-jp/updates/public-preview-azure-key-vault-csi-driver-support-in-azure-kubernetes-service/)<br>
secrets store csi driverでAKSから keyvault に格納されているシークレットを参照できる。

[Public preview: Automatic Cluster Upgrades in AKS](https://azure.microsoft.com/ja-jp/updates/public-preview-automatic-cluster-upgrades-in-aks/)<br>
クラスタの自動アップグレードがパブリックプレビュー

[Release 2021-01-25](https://github.com/Azure/AKS/releases/tag/2021-01-25)<br>
AKS の定期更新。Application Gateway Ingress Controller　が GA になったり。

###  Service Bus
[Announcing general availability support for Java Message Service (JMS) 2.0 API on Azure Service Bus Premium](https://azure.microsoft.com/en-us/updates/announcing-general-availability-support-for-java-message-service-jms-20-api-on-azure-service-bus-premium/)

### IoT hub
[Action Required: Upgrade IoT Hub IP filter before 1 February 2022](https://azure.microsoft.com/ja-jp/updates/action-required-upgrade-iot-hub-ip-filter-before-1-february-2022/)

### IoT Edge
[Azure IoT Edge for Linux on Windows available for public preview](https://azure.microsoft.com/ja-jp/updates/azure-iot-edge-for-linux-on-windows-available-for-public-preview/)

### Cosmos DB
[Azure Cosmos DB: Multiple region Availability Zones support with single-region write now generally available](https://azure.microsoft.com/ja-jp/updates/azure-cosmos-db-multiple-region-availability-zones-support-with-singleregion-write-now-generally-available/)
Cosmos DB の AZ サポート

[Azure Cosmos DB Management with PowerShell cmdlets is now generally available](https://azure.microsoft.com/ja-jp/updates/azure-cosmos-db-management-with-powershell-cmdlets-is-now-generally-available/)<br>
Cosmos DB 操作用の Powershell Cmdlet がGA

[LIKE keyword support now generally available in Azure Cosmos DB](https://azure.microsoft.com/ja-jp/updates/like-keyword-support-now-generally-available-in-azure-cosmos-db/)<br>
LIKE キーワードサーチがサポートされた

### SQL DB
[Confidential computing using Always Encrypted with secure enclaves now in public preview](https://azure.microsoft.com/ja-jp/updates/confidential-computing-using-always-encrypted-with-secure-enclaves-now-in-public-preview/)<br>
詳細は、[こちら](https://techcommunity.microsoft.com/t5/azure-sql/always-encrypted-with-secure-enclaves-in-azure-sql-database/ba-p/2051544)

### HD Insights
[General availability: Azure HDInsight extends capabilities for encryption of data in transit and at rest](https://azure.microsoft.com/ja-jp/updates/azure-hdinsight-extends-capabilities-for-encryption-of-data-in-transit-and-at-rest/)

### Cognitive
[Azure Cognitive Services Translator - Inuktitut language text translation now available](https://azure.microsoft.com/ja-jp/updates/azure-cognitive-services-translator-inuktitut-language-text-translation-now-available/)<br>
カナダのイヌイット語が使えるようになったとか。ほう。

[General availability: Text Analytics' Named Entity Recognition v3 now supports 10 languages with improved AI quality](https://azure.microsoft.com/ja-jp/updates/text-analytics-ner-improved-ai-quality/)<br>

### Machine Learning
[General availability: Azure Machine Learning Output Datasets](https://azure.microsoft.com/ja-jp/updates/general-availability-azure-machine-learning-output-datasets/)

[Public preview: Azure Machine Learning Data Labeling – Image Instance Segmentation](https://azure.microsoft.com/ja-jp/updates/public-preview-azure-machine-learning-data-labeling-image-instance-segmentation/)

### Other
[Azure Sphere OS version 21.01 is now available for evaluation](https://azure.microsoft.com/ja-jp/updates/azure-sphere-os-version-2101-is-now-available-for-evaluation/)<br>
[Azure Sphere version 21.01 is now available](https://azure.microsoft.com/ja-jp/updates/azure-sphere-version-2101-is-now-available/)<br>
Azure Sphere OS 21.01 がリリース

[The Azure Quota REST API to manage service limits (quotas) is now generally available](https://azure.microsoft.com/ja-jp/updates/azure-quota-rest-apis-to-manage-service-limits-quota-are-now-generally-available/)<br>
クォータ操作系の API が GA

[Azure Cost Management and Billing updates – January 2021](https://azure.microsoft.com/en-us/blog/azure-cost-management-and-billing-updates-january-2021/)<br>
コストマネジメント系のエクスペリエンスの改善とか

[Azure Boards - New enhancements to Delivery Plans 2.0](https://azure.microsoft.com/ja-jp/updates/azure-boards-new-enhancements-to-delivery-plans-20/)