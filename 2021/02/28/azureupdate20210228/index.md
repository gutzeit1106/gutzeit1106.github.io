# Azure Update - 2021.2.28


来週は ignite ですね。今週はやたらと廃止系が多い。

### Azure AD
[Please upgrade your Azure AD Connect sync to a newer version by 29 February 2024](https://azure.microsoft.com/en-us/updates/please-upgrade-your-azure-ad-connect-sync-to-a-newer-version-by-29-february-2024/)
バージョン 1.1.751.0 以前 の Azure AD Connect は2024年2月29日に廃止される

[Email one-time passcode authentication on by default starting October 2021](https://azure.microsoft.com/en-us/updates/email-onetime-passcode-authentication-on-by-default-starting-october-2021/)

### VM
[Automatic Azure VM extension upgrade capabilities now in public preview](https://azure.microsoft.com/en-us/updates/automatic-extension-upgrade-now-in-preview/)<br>
Azure VM拡張機能を自動的にアップグレードする機能がパブリックプレビュー。マイナーバージョンは前から自動更新可能だった記憶があるのでメジャーバージョンのアップグレードかな。

[The G5 and GS5 Azure VMs will no longer be hardware-isolated on 28 February 2022](https://azure.microsoft.com/en-us/updates/the-g5-and-gs5-azure-vms-will-no-longer-be-hardwareisolated-on-28-february-2022/) <br>
2022年2月28日以降、G5およびGS5 Azure VMはハードウェアで分離されなくなり、G5 および GS5 AzureVM でハードウェアが独立していることは保証されなくなる。以降は dedicated host を使おう。

[The E64i_v3 & E64is_v3 Azure VMs will not be hardware-isolated on 28 February 2022](https://azure.microsoft.com/en-us/updates/the-e64iv3-e64isv3-azure-vms-will-not-be-hardwareisolated-on-28-february-2022/)

[The GS5 Azure Virtual Machines will no longer be SAP HANA Certified on 28 February 2022](https://azure.microsoft.com/en-us/updates/the-gs5-azure-virtual-machines-will-no-longer-be-sap-hana-certified-on-28-february-2022/)<br>
GS5 VM は 2022年2月28日以降 SAP HANA 認定じゃなくなる。E64ds_v4が推奨。

[Azure Image Builder Service now generally available](https://azure.microsoft.com/en-us/updates/azure-image-builder-service-now-generally-available/)

### AKS
[General availability: Enabling IBM WebSphere on Azure Kubernetes Service](https://azure.microsoft.com/en-us/updates/enabling-ibm-websphere-on-azure-kubernetes-service/)<br>
AKS 上で IBM WebSphere を有効化することが GA になった

[AKS legacy Azure AD integration will be retired on 29 February 2024](https://azure.microsoft.com/en-us/updates/aks-legacy-azure-ad-integration-will-be-retired-on-29-february-2024/)

[General Availability: Azure Red Hat OpenShift support for OpenShift 4.6](https://azure.microsoft.com/en-us/updates/general-availability-azure-red-hat-openshift-support-for-openshift-46/)

### Storage
[Action required: Switch to Azure Data Lake Storage Gen2 by 29 February 2024 ](https://azure.microsoft.com/en-us/updates/action-required-switch-to-azure-data-lake-storage-gen2-by-29-february-2024/) on 2021-02-25<br>
2024年2月29日にAzure Data Lake StorageGen 1 が廃止。Data Lake Gen 2 に切り替える必要がある。 

### Site Recovery
[Azure Site Recovery update rollup 54 is now generally available - February 2021](https://azure.microsoft.com/en-us/updates/site-recovery-update-rollup-54-february-2021/) on 2021-02-26 <br>

[Cross Region Restore of Azure VMs now generally available](https://azure.microsoft.com/en-us/updates/cross-region-restore-of-azure-vms-now-generally-available/)<br>

### Migrate
[We are retiring Classic Azure Migrate on 29 February 2024](https://azure.microsoft.com/en-us/updates/we-are-retiring-classic-azure-migrate-on-29-february-2024/) on 2021-02-25<br>
2024年2月29日に Classic Azure Migrate が廃止されることがアナウンス。

### Application Gateway
[Azure Application Gateway analytics will be retired on 29 February 2024](https://azure.microsoft.com/en-us/updates/azure-application-gateway-analytics-will-be-retired-on-29-february-2024/)<br>
Application Gateway analytics が　2024年2月29日　に廃止。

### Front Door
[Azure Front Door Standard and Premium now in public preview](https://azure.microsoft.com/en-us/updates/azure-front-door-standard-and-premium-now-in-public-preview/)

[Web Application Firewall Integration with Azure Front Door Standard and Premium SKU](https://azure.microsoft.com/en-us/updates/web-application-firewall-integration-with-azure-front-door-standard-and-premium-sku/)

[General availability: Web Application Firewall for Azure Front Door managed ruleset refresh](https://azure.microsoft.com/en-us/updates/web-application-firewall-for-azure-front-door-managed-ruleset-refresh/)

### Azure Firewall
[Azure Firewall Premium now in public preview](https://azure.microsoft.com/en-us/updates/azure-firewall-premium-now-in-public-preview/)<br>
Azure Firewall Premium が public preview になった。TLS検査や、IDPS、webカテゴリ、URLフィルタリングなどが使用できる。

### Key Vault
[Azure role-based access control (RBAC) for Azure Key Vault data plane authorization is now generally available](https://azure.microsoft.com/en-us/updates/azure-rolebased-access-control-rbac-for-azure-key-vault-data-plane-authorization-is-now-available/) on 2021-02-16<br>
Key Vault への Data plane　へのアクセスが RBAC で制御可能になった。GA。

### Monitor
[Start using new alerts in Azure Monitor for Azure China 21Vianet ](https://azure.microsoft.com/en-us/updates/start-using-new-alerts-in-azure-monitor-for-azure-china-21vianet/) on 2021-02-25 <br>

[Start using new alerts in Azure Monitor for Azure Government Cloud ](https://azure.microsoft.com/en-us/updates/start-using-new-alerts-in-azure-monitor-for-azure-government-cloud/) on 2021-02-25<br>

[We’re retiring Classic Application Insights on 29 February 2024](https://azure.microsoft.com/en-us/updates/we-re-retiring-classic-application-insights-on-29-february-2024/)

[We’re retiring Azure Network Watcher Connection Monitor (classic) on 29 February 2024](https://azure.microsoft.com/en-us/updates/we-re-retiring-azure-network-watcher-connection-monitor-classic-on-29-february-2024/)

[We’re retiring Network Performance Monitor on 29 February 2024](https://azure.microsoft.com/en-us/updates/we-re-retiring-network-performance-monitor-on-29-february-2024/)

[.NET Core support for Analysis Services client libraries is generally available](https://azure.microsoft.com/en-us/updates/net-core-support-for-analysis-services-client-libraries-is-ga/)<br>
Azure Analysis Services Client Library の　.NETCore サポート版がGA

[New disk bursting metrics](https://azure.microsoft.com/en-us/updates/new-disk-bursting-metrics/)

[Public preview: Application Insights auto-instrumentation for .NET5 App Services](https://azure.microsoft.com/en-us/updates/public-preview-application-insights-autoinstrumentation-for-net5-app-services/)

[Application Insights availability troubleshooting report for URL tests](https://azure.microsoft.com/en-us/updates/application-insights-availability-troubleshooting-report-for-url-tests/)

[Generally available: Application Insights synthetic monitoring SLA report template](https://azure.microsoft.com/en-us/updates/generally-available-application-insights-synthetic-monitoring-sla-report-template/)

[Public preview: Application Insights work item integration for Azure DevOps & GitHub](https://azure.microsoft.com/en-us/updates/public-preview-application-insights-work-item-integration-for-azure-devops-github/)

[Datadog integration with Azure: Public Preview](https://azure.microsoft.com/en-us/updates/datadog-integration-with-azure-public-preview/)

[Earlier versions of Analysis Services client libraries will retire on 30 June 2021](https://azure.microsoft.com/en-us/updates/earlier-versions-of-analysis-services-client-libraries-will-retire-on-30-june-2021/)

### App Service 
[Community support for Python 3.6 is ending on 23 December 2021](https://azure.microsoft.com/en-us/updates/community-support-for-python-36-is-ending-on-23-december-2021/)<br>
WindowsおよびLinuxでのPython3.6の延長サポートが2021年12月23日に終了することに伴い、App Service でもサポート終了。

### API Management
[API Management Diagnostics now available in public previe](https://azure.microsoft.com/en-us/updates/api-management-diagnostics-now-available-in-public-preview/)<br>
App Service でできるようないろんなトラブルシューティング解析がポータルからできるようになった。

### Batch
[Azure Batch rendering VM images & licensing will be retired on 29 February 2024](https://azure.microsoft.com/en-us/updates/azure-batch-rendering-vm-images-licensing-will-be-retired-on-29-february-2024/) on 2021-02-24 <br>
Azure Batch のノードプールでは、現在グラフィックスおよびレンダリングアプリケーションがプリインストールされた Marketplace VM イメージを使用できるが、これらのVMイメージと有料ライセンスが、2024年2月29日以降使用できなくなる。以後はカスタムイメージを作成する必要がある。

[Azure Batch ‘CloudServiceConfiguration’ pools will be retired on 29 February 2024](https://azure.microsoft.com/en-us/updates/azure-batch-cloudserviceconfiguration-pools-will-be-retired-on-29-february-2024/)<br>
Azure Batch の CloudServiceConfiguration pool　が 2024年2月29日 でリタイア。VMSS プールを使っていくことになる。

### Service Bus
[General availability support for Java Message Service (JMS) 2.0 API on Azure Service Bus Premium](https://azure.microsoft.com/en-us/updates/announcing-ga-support-for-java-message-service-jms-20-api-on-azure-service-bus-premium/)

### Media Service
[Update your Azure Media Services REST API and SDKs to v3 by 29 February 2024](https://azure.microsoft.com/en-us/updates/update-your-azure-media-services-rest-api-and-sdks-to-v3-by-29-february-2024/)

### Cognitive
[We’re retiring the standard version of Custom Voice on 29 February 2024](https://azure.microsoft.com/en-us/updates/we-re-retiring-the-standard-version-of-custom-voice-on-29-february-2024/) on 2021-02-24<br>
Custom Voice のスタンダードバージョンが 2024年2月29日に廃止。

[We are retiring Azure Cognitive Services Text Analytics v2.x on 29 February 2024](https://azure.microsoft.com/en-us/updates/we-are-retiring-azure-cognitive-services-text-analytics-v2x-on-29-february-2024/) on 2021-02-24<br>
ext Analytics v2.x が 2024年2月29日に廃止

[Azure Batch Transcription and Customization Rest API v2 will be retired by 29 February 2024](https://azure.microsoft.com/en-us/updates/azure-batch-transcription-and-customization-rest-api-v2-will-be-retired-by-29-february-2024/)　on 2021-02-24 <br>
Azure Batch Transcription and Customization Rest API v2が、2024年2月29日に廃止。

[General availability: Azure Cognitive Services Translator releases nine new languages](https://azure.microsoft.com/en-us/updates/azure-cognitive-services-translator-releases-nine-new-languages/)

### CosmosDB
[Update the Azure Cosmos DB Java SDK by 29 February 2024](https://azure.microsoft.com/en-us/updates/update-the-azure-cosmos-db-java-sdk-by-29-february-2024/) on 2021-02-25<br>
AzureCosmos DB JavaSDK の Sync 2.xバージョンが廃止

[More ways to use composite indexes in Azure Cosmos DB now generally available](https://azure.microsoft.com/en-us/updates/more-ways-to-use-composite-indexes-in-azure-cosmos-db-now-generally-available-2/)<br>

### SQL
[Log Replay Service for Azure SQL Managed Instance in public preview](https://azure.microsoft.com/en-us/updates/log-replay-service-for-azure-sql-managed-instance-in-public-preview/)

[Additional IOPS feature for MySQL - Flexible Server in preview ](https://azure.microsoft.com/en-us/updates/additional-iops-feature-for-mysql-flexible-server-in-preview-4/)

[Azure Database for PostgreSQL ending support for PostgreSQL version 9.6 on 11 November 2021](https://azure.microsoft.com/en-us/updates/azure-database-for-postgresql-ending-support-for-postgresql-version-96-on-11-november-2021/)

[Azure Database for PostgreSQL – Hyperscale (Citus) audit logging via pgAudit now in public preview](https://azure.microsoft.com/en-us/updates/azure-database-for-postgresql-hyperscale-citus-audit-logging-via-pgaudit-now-in-public-preview/)

### Power BI
[Power BI Connector for Azure Databricks is now generally available](https://azure.microsoft.com/en-us/updates/power-bi-connector-for-azure-databricks-is-now-generally-available/) on 2021-02-26<br>
Power BI Connector for AzureDatabrick が GA

### Data Factory 
[General availability: Data flows in Azure Data Factory and Azure Synapse now support Reserved Instances](https://azure.microsoft.com/en-us/updates/general-availability-data-flows-in-azure-data-factory-and-azure-synapse-now-support-reserved-instances/)

### HD Insight
[Azure Dav4-series VMs are available in Azure HDInsight](https://azure.microsoft.com/en-us/updates/azure-dav4series-vms-are-available-in-azure-hdinsight/)

### Machine Learning
[General availability: Azure Machine Learning updates for native terminal](https://azure.microsoft.com/en-us/updates/general-availability-azure-machine-learning-updates-for-native-terminal/)

### IoT
[Azure IoT Edge 1.1.0 release is now generally available](https://azure.microsoft.com/en-us/updates/iot-edge1-1-0/)

[Azure Sphere OS version 21.02 is now available for evaluation](https://azure.microsoft.com/en-us/updates/azure-sphere-os-version-2102-is-now-available-for-evaluation/)

### Azure Stack
[Azure Stack Edge Pro FPGA is retiring on 29 February 2024](https://azure.microsoft.com/en-us/updates/azure-stack-edge-pro-fpga-is-retiring-on-28-february-2024/) on 2021-02-25<br>
Azure Stack Edge ProFPGA が 2024年2月29日 に廃止

### Other
[Microsoft plans to establish first datacenter region in Indonesia](https://azure.microsoft.com/en-us/updates/microsoft-plans-to-establish-first-datacenter-region-in-indonesia/) on 2021-02-24<br>
インドネシアにデータセンターができる

[Update your scripts to use Az PowerShell modules by 29 February 2024](https://azure.microsoft.com/en-us/updates/update-your-scripts-to-use-az-powershell-modules-by-29-february-2024/)<br>
AzureRM Module が 2024年2月29日 に廃止

[Jenkins plug-ins for Azure are being retired on 29 February 2024](https://azure.microsoft.com/en-us/updates/jenkins-plugins-for-azure-are-being-retired-on-29-february-2024/)