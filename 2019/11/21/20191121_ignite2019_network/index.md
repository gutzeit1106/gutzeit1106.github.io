# Microsoft Ignite 2019 で発表された新機能のまとめ(Network編)


Microsoft Ignite 2019 の Network 系サービスの update まとめです。
4つのカテゴリに分けて各種サービスの update が紹介されていました。

・Connect & Extend<br>
・Protect<br>
・Deliver<br>
・Monitoring<br>

ネットワークサービスは、難しいですね。機能ごとのセッションを追いきれなかったので、補足とかある人は [twitter](https://twitter.com/y10exxx) とかでご教示いただきたいなぁ。

## 1. Connect & Extend
### Azure Peering Service (Preview)
ISP を介してパブリックネットワーク経由で Microsoftクラウド(Office 365、Dynamics 365、Azure など)に最適かつ信頼性の高いルーティングを提供するサービスです。
専用接続を使用して、Microsoftクラウドにセキュアなネットワーク接続をおこなうサービスとしては、Azure ExpressRoute　がありましたが、Peering Service では、プライベート回線ではなくパブリックネットワークが利用されます。<br>
日本でも、NTT コミュニケーションズや IIJ が早くも対応サービスを公表していました。

Azure Peering Service<br>
https://docs.microsoft.com/ja-jp/azure/peering-service/

### Azure Virtual WAN
Azure Virtual WAN は、Azureを介して拠点間接続を行うことができる大規模拠点間接続サービスです。
Ignite 2019 では、プレビューだった　ExpressRoute と P2S VPN　接続との統合が一般提供となりました。

Azure Firewall や Azure Virtual WAN に存在する各リージョンのハブとハブの間で接続させる機能についてのプレビュー開始が紹介されていました。

それに伴い公式ドキュメントも更新されているようです。

Azure Virtual WAN の概要<br>
https://docs.microsoft.com/ja-jp/azure/virtual-wan/virtual-wan-about

### ExpressRoute
オンプレミスからのネットワークトラフィックを Azure 上の仮想ネットワークゲートウェイをバイパスできる FastPath のオプションが GA になりました

ExpressRoute 仮想ネットワーク ゲートウェイと FastPath<br>
https://docs.microsoft.com/ja-jp/azure/expressroute/expressroute-about-virtual-network-gateways#fastpath

また、ExpressRoute Direct で使用できる ExpressRoute Local が GA になったようです。
ExpressRoute Local では、アクセス可能なリージョンが限定されていますが、データ転送料金などが追加で発生ない SKU らしいです。ただ、ExpressRoute Global Reachも使えないみたいですね。

ExpressRoute Local<br>
https://docs.microsoft.com/ja-jp/azure/expressroute/expressroute-faqs#expressroute-local

ネットワークデバイスと Microsoft のネットワーク デバイスの間の物理リンクと MACSec(Media Access Control Security)で暗号化する機能がプレビューで紹介されていました。ExpressRoute Direct のみ利用可能のようです。

MACsec によるポイントツーポイント暗号化に関する FAQ <br>
https://docs.microsoft.com/ja-jp/azure/expressroute/expressroute-about-encryption#point-to-point-encryption-by-macsec-faq

### ExpressRoute for satellites (GA)
衛星接続とExpressRouteを通じてMicrosoft サービスに接続できる[Azure ExpressRoute for satellites](https://azure.microsoft.com/de-de/updates/azure-expressroute-for-satellites-is-now-available/) が GA となりました。

Satellite connectivity expands reach of Azure ExpressRoute across the globe <br>
https://azure.microsoft.com/en-us/blog/satellite-connectivity-expands-reach-of-azure-expressroute-across-the-globe/

### VPN
S2S 接続では、10Gbpsの帯域の Gateway タイプと、全ての Gateway タイプでIKEv1とIKEv2のサポートがGAになったようです。
VPN Gateway でのパケットパキャプチャ採取も開発しているらしいです。これは早く GA なってほしいですね。

P2S 接続では、Azure ADと多要素認証のサポート、Windows アプリの Azure VPN Clinetを開発しているみたいです。

### IPv6 in Azure VNETs 
Azure の仮想ネットワーク内で、IPv4 と IPv6 の両方がサポートされるようになりました。GA らしいです。
ただ、ドキュメントはまだ更新されていないので、今後反映されていくものと思われます。

What is IPv6 for Azure Virtual Network? (Preview)<br>
https://docs.microsoft.com/en-us/azure/virtual-network/ipv6-overview


## 2. Protect

### Azure Private Link (Preview)
Azure Private Link が、プレビューで公開されました。Azure Storage や SQL、Cosmos DB などの PaaS リソースに対して、仮想ネットワーク内のプライベート IP を経由して接続できる機能になります。

Azure Private Link <br>
https://docs.microsoft.com/ja-jp/azure/private-link/

### Azure Firewall Manager (Preview)
Azure Firewall Manager という機能がプレビューで公開されました。
リージョンやサブスクリプションをまたがる複数のAzure Firewall インスタンスを一元的にセキュリティポリシーを展開、管理できるみたいです。

Azure Firewall Manager is now in preview <br>
https://azure.microsoft.com/ja-jp/updates/azure-firewall-manager-is-now-in-preview/

### Azure Bastion
JumpBox の PaaS のような、サーバーをもたずに仮想ネットワークから Azure VM に RDP や SSH ができるようになる Azure Bastion が GA になりました。
プレビューから GA まで早かったですね。　

Azure Bastion とは<br>
https://docs.microsoft.com/ja-jp/azure/bastion/bastion-overview

### Azure WAF
Azure Front Door や Azure Application Gateway と統合した WAF サービスになったという話でした。(updateなのか？)URIなどで WAF ポリシーをカスタマイズできたり地理レベルでフィルタリングを適用したりするような機能が開発中らしいです。

## 3. Deliver

### Application Gateway
Application Gateway のところで発表するの？って感じでしたが、AKS の Application Gateway Ingres Controleer が GA しました。<br>
他には、Application GatewayとAzure Keyvaltが統合され、SSL 証明書を管理する機能がGAになりました。レイテンシやバックエンドのエラーコードなどメトリックも拡充したらしいです。

Comming Soonとしては、リスナーにワイルドカードが許容されるようになりサブドメインごとにリスナー作成が不要になる Wildcard listener が開発中らしいです。

### Azure Front Door
内部動作の update のようでしたが、HTTPパフォーマンスが向上したり、DDos対策などでセキュリティ性も向上しているようです。

### Azure CDN
Azure Storage や VM、Media Serviceから Microsoftの Azure CDN　のエグレストラフィックは無料になりました。
CDN ルールのカスタマイズ性や容易さを向上できるように開発を進めているとのこと。

## 4. Monitoring

### Internet Analyzer(Preview)
Azure Internet Analyzerがプレビューで公開されました。次のようなことができるらしい。

・web アプリをクラウドに移行した時のインパクト計測<br>
・Azure Front DoorやCDNを使用したときのパフォーマンス影響の計測<br>
・2種類のweb appやマルチリージョンのアプリを配置したときのパフォーマンスなどを計測<br>

Internet Analyzer<br>
https://azure.microsoft.com/ja-jp/services/internet-analyzer/


### Azure Monitor for networks
ネットワーク周りのモニタリング、メトリック計測などが向上、拡充したそうです。
Traffic 分析の処理速度向上やロードバランサーやグローバルピアリング、UDRなどの接続性チェックが向上が挙げられていました。

今後としては、クラウドネットワーク全体の正常性を確認できるコンソールやエージェント、設定レスな機能の開発を進めているらしいです。　
