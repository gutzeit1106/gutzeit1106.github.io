# Azure Update - 2021.Jan


定期的に Azure 関係の update をまとめてポストしていこうかと思います。内容が膨大になりそうなのでおそらく特に興味ないやつはあっさり or ばっさり。Azure Monitor 関係の更新が多い。

### Compute
[NCas_T4_v3-Series VMs are now generally available](https://azure.microsoft.com/ja-jp/updates/ncast4v3series-vms-are-now-generally-available/)   
NVIDIA T4 GPU搭載のNCas_T4_v3 サイズが West US2, West Europe,  Korea Central リージョンで GA。 

[Upgrade Azure Service Fabric Clusters* on Unsupported Versions Below 6.3.63.* By January 19, 2021](https://techcommunity.microsoft.com/t5/azure-service-fabric/upgrade-azure-service-fabric-clusters-on-unsupported-versions/ba-p/2041278)   
もうサポートされないような古い Service Fabric のクラスタバージョンを使用している場合は、1/19 に破壊的変更があるので早く更新しなさいという通告。
### Storage
[Public preview | Azure NetApp Files : Application Consistent Snapshot tool (AzAcSnap)](https://azure.microsoft.com/ja-jp/updates/public-preview-azure-netapp-files-application-consistent-snapshot-tool-azacsnap/)  
Azure NetApp 用のアプリケーション整合性スナップショットツール (AzAcSnap)がパブリックプレビュー。Linux環境で使用できるらしい。

### Azure Network
[Public IP SKU upgrade generally available](https://azure.microsoft.com/ja-jp/updates/public-ip-sku-upgrade-generally-available/)    
Azure の Public IP アドレスリソースが Basic から Standard の SKU にシームレスに更新できるようになった。GA。

### Azure Policy
[Built-in Azure Policy support for NSG Flow Logs is now available](https://azure.microsoft.com/ja-jp/updates/nsg-flow-logs-built-in-azure-policy/)
Azure Policy のビルトインポリシーで NSG Flow Logs の構成がサポートされるようになった。
Audit(監査警告)や DeployIfNotExists (ポリシー違反の場合自動設定) ができる。

### Security
[Azure Defender for SQL (in Azure Security Center)—News and updates for December 2020](https://azure.microsoft.com/ja-jp/updates/defender-for-sql-december2020/)   
Azure Defender for SQL の 2020年12月分の更新まとめ。Azure Defender for SQL serversがGAらしい。

[Azure Security Center—News and updates for December 2020](https://azure.microsoft.com/ja-jp/updates/asc-december2020/)   
Azure Security Center の 2020年12月分の更新まとめ。

### Monitor
[Monitoring your Azure Data Explorer Clusters with Azure Monitor (Insights) - public preview](https://azure.microsoft.com/ja-jp/updates/monitoring-azure-data-explorer-clusters-with-azure-monitor/)   
Azure Monitor for Azure Data Explorer がプレビュー

[Activity Log Alerts - Change in "Description" property in the alert payload now in development](https://azure.microsoft.com/ja-jp/updates/activity-log-alerts-change-in-description-property-in-the-alert-payload/)  
Activity Log Alerts が発報されたときのデータペイロード変わる。2021 年 4 月 1 日より、Description のプロパティにアラートルールの説明が含まれる。

[Cross resource query between Azure Monitor and Azure Data Explorer in public preview](https://azure.microsoft.com/ja-jp/updates/cross-resource-query-between-azure-monitor-and-azure-data-explorer-in-public-preview/)   
Azure Monitor と Azure Data Explorer 間でクロスクエリーができる機能がプレビュー。

[View change data through Azure Monitor workbook integration with Application Change Analysis](https://azure.microsoft.com/ja-jp/updates/view-change-data-through-azure-monitor-workbook-integration-with-application-change-analysis/)   
Azure Monitor のワークブックで "Application Change Analysis"のデータと統合してグラフを作成できるようになる。 

### Other
[View changes at-scale with Application Change Analysis now in public preview](https://azure.microsoft.com/ja-jp/updates/at-scale-change-view/)  
Azure ポータルの "Application Change Analysis" の項目から参照できる、アプリケーション変更分析の機能がパブリックプレビュー。
Azure Resource Graph の情報をベースに、Azure リソースの構成、状態変更など時系列的な変化を参照できる。現段階で Azure Lighthouse で委任されたサブスクリプションは追跡できない制限があるらしい。

[Azure Health Bot is now generally available](https://azure.microsoft.com/ja-jp/updates/azure-health-bot/)  
[Azure Health Bot](https://azure.microsoft.com/en-us/services/bot-services/health-bot/) というサービスが GA。[ドキュメント](https://docs.microsoft.com/en-us/healthbot/)を見るに医療機関のシステム開発者向けに、医療業界のコンプライアンスや規制要件に対応する bot 作成を容易化してくれるサービスらしい。

### 参照
[Azure Update](https://azure.microsoft.com/ja-jp/updates)  
[Tech CommunityTech Community](https://techcommunity.microsoft.com/t5/azure/ct-p/Azure)