# Microsoft Ignite 2019 で発表された新機能のまとめ(AKS/Container編)


Microsoft Ignite 2019 で AKS / Container　関連も多く update がありました。
画期的というよりは、実用的な機能が充実してきたと思える機能が多くありました。

### 1. Threat protection for Azure Kubernetes Service
Azure Security Centerでの、AKS クラスタを対象とした保護機能が拡大しているようです。
次の項目が挙げられていました。

• Discovery & Visibility - サブスクリプション内のAKSインスタンスの継続的ディスカバリ<br>
• Recommendations - セキュリティベストプラクティスを実行を助ける項目提示<br>
• Threat Detection - ホストとクラスターベースの分析<br>

### 2. Streamlined developer experience for Azure Kubernetes Service
Dev Spacesで、開発マシンとAKS Clusterを接続して、AKS - 開発マシン間のトラフィックをリダイレクトしてあたかも同一のAKSクラスターに開発マシンのコードがあるかのように開発できるらしい。すげー。

Connect your development machine to an AKS cluster (preview) <br>
https://docs.microsoft.com/en-us/azure/dev-spaces/how-to/connect

Github Actionと連携してプルリクエストをメインブランチにマージする前に変更点をAKSクラスタで直接テストできるらしい。

GitHub Actions & Azure Kubernetes Service (preview)<br>
https://docs.microsoft.com/en-us/azure/dev-spaces/how-to/github-actions

### 3. Kubernetes clusters with Azure Availability Zone, Cluster autoscaler and multiple node pools
AKS Clusterでの可用性ゾーン構成とマルチノードプールとクラスターオートスケーラーが正式にGAしました。
正統なアップデートって感じがします。エージェントノードの構成がより柔軟になりそうです。
この構成を構築するにはAzure CLIを最新にする必要があったので注意。

ドキュメントとしては、Githubの[Release 2019-10-28](https://github.com/Azure/AKS/releases/tag/2019-10-28)が詳しそうです。

### 4. Improved Azure Kubernetes Service security with authenticated IP
AKS上のkube-apiserverへの接続IP制限をかける機能がGAされました。
前のAzure Firewallを使用した手順から、Standard LoadBlancerと統合された機能になったみたいですね。これは嬉しい。

Secure access to the API server using authorized IP address ranges in Azure Kubernetes Service<br>
https://docs.microsoft.com/en-us/azure/aks/api-server-authorized-ip-ranges)

### 5. Azure Kubernetes Service – easier diagnostics and logging
運用中のAKS ClusterのトラブルシューティングをおこなうAKS　Diagnosticsというプレビュー機能が発表されました。
派手な新機能に着目しがちだけどこういう実用的な機能めちゃくちゃ大事だと思う。

Azure Kubernetes Service（AKS）診断の概要<br>
https://docs.microsoft.com/en-us/azure/aks/concepts-diagnostics

AKS Clusterのノードやpodの問題を診断して、Azureのblobストレージに結果を出力するaks-periscopeというツールも紹介されています。。

aks-periscope<br>
https://github.com/Azure/aks-periscope

### 6. Open Container Initiative artifacts support in Azure Container Registry
Docker ImageとHelm Chartに加えて、OCI形式のアーティファクトやイメージもACRで保存できる機能が正式にGAとなりました。

### 7. Azure Security Center vulnerability assessments available for Azure Container Registry
Azure Security CenterでAzure Container Registryの脆弱性評価と推奨事項もできるようになるというおはなし。

### 8. Azure Monitor for containers: Preview of Hybrid Monitoring, general availability of Prometheus Support
オンプレのK8sクラスタととAzure上のAKSクラスタをAzure Monitorでハイブリッドに監視できる機能がプレビューで発表されました。
また、PrometheusからメトリックやログをAzure Monitorに取り込む機能がGAとなりました。

### 9. On-demand pricing and Azure Monitor Log Analytics integration for Azure Red Hat OpenShift
以前まで Azure Redhat Openshiftをデプロイする際は、 reserved instance を事前に購入しておく必要がありましたが、今回の発表で[時間単位の課金](https://azure.microsoft.com/ja-jp/updates/azure-red-hat-openshift-hourly-prices/)でデプロイが可能になりました。また、Red Hat OpenShiftクラスタと Azure Monitor Log Analyticsの統合がプレビューだそうです。
