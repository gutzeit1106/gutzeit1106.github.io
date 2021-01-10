# Service Fabric Managed Cluster (Preview)  を試してみた #1


# はじめに
Azure Service Fabric は、Azure SQL DB, Event Hub, Microsoft Teamsなどの Azure のコアインフラストラクチャやミッションクリティカルなサービスを提供する基盤テクノロジとしても利用されている Azure サービスになります。   
2020/9/28 に [Azure Service Fabric Managed Cluster](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-managed-clusters-are-now-in-public-preview/ba-p/1721572)が Public Previewとなりました。   
今回は実際にリソースを作成して既存の Service Fabricとの違いなどを整理していきたいと思います。

# Service Fabric Managed Cluster とは
Service Fabric は VM Scaleset を中心にディスクや仮想ネットワークやロードバランサーといった各種 Azure リソースが組み合わされて提供されるサービスです。   
そのため従来では、そういった関連リソースの管理についてもユーザが担う必要があり、例えば Azure Resource Manager テンプレートなどで Service Fabric をデプロイし、構成更新などをおこなう場合、全ての関連リソースの定義を矛盾なく定義する必要があり、かなりのコストが必要でした。    
Service Fabric Managed Cluster では、ユーザは"Managed Cluster"という単一のリソースに集約して管理することで関連する各 Azure リソース をユーザが管理する必要がなくなりました。   
従来の Service Fabric では、関連リソースを含めるとシンプルな構成でも 1000 行近い定義を記述する必要がありましたが、Managed Cluster では 100 行ほどに圧縮されます。   
Service Fabric のトラブルの多くが関連リソースに対する不適切な構成や操作を起因したもので"Managed Cluster"という抽象化レイヤーを挟むことで管理を簡潔化し不要なトラブルを軽減できることを見込んでいます。   
以下は概念図となります。従来は各種 Resource Provider (RP) 毎の操作をユーザが管理する必要がありましたが、Managed Cluster では Service Fabric RP を経由した操作になり直接構成リソースを操作することがないため不適切な構成や操作のリスクを軽減できます。

** 従来の Service Fabric Cluster  **
![sfrp-composition-resource.png](/img/2021/Jan/ManagedServiceFabric/751df155-c525-be4f-7ae3-45dba3cf027c.png)

** Service Fabric Managed Cluster **
![sfrp-encapsulated-resource.png](/img/2021/Jan/ManagedServiceFabric/e1b3e820-6b5b-dd6d-09b4-7326dfd9e6a3.png)

Service Fabric の Tech Community では次のようなポイントが Managed Cluster の特徴として挙げられていました。クラスター証明書の更新が完全 Managed になったのもうれしいですね。

1. Encapsulated Resource Model   
   クラスターを構成する個別のリソース (VM、ストレージ、ネットワーク構成など) をすべて定義しなくてもクラスターを作成できます。

2. Storage backed by managed disks   
   VM に付属する一時記憶域のサイズに制限されなくなりました。これで、アプリケーションのニーズを満たすストレージの量を簡単に選択できるようになりました。

3. Fully managed cluster certificates   
   クラスター証明書は Azure によって完全に管理されるようになったので、期限切れのクラスター証明書などの心配が必要ありません。

4. Single step cluster operations   
   以前に複数のステップが必要なノード・タイプの除去などの操作を、単一ステップで完了できるようになりました。Service Fabric の管理対象クラスターは、要求を満たすために必要な変更を自動的に行い、プロセス中のエラーをより適切に処理します。

4. Enhanced cluster safety   
   クラスター操作は、Service Fabric リソース プロバイダーによって検証され、安全に実行できることを確認します。

5. Simplified Cluster SKUs   
  テスト環境と実稼働環境の作成に役立つ 2 つの新しいクラスター SKU (基本、標準)。標準 SKU を使用する場合、クラスター内の使用可能なリソースを最大限に活用するために、耐久性と信頼性の値が自動的に調整されます。

# テンプレート
Azure Powershell などからもデプロイが可能ですが、今回はリソース構成を正確に把握したいので ARM Template を使ってデプロイしようと思います。[サンプルテンプレート](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/SF-Managed-Basic-SKU-1-NT)が提供されているのでこちらを使いますが、ちょっとazuredeploy.json の中身を見てみます。

```SF-Managed-Basic-SKU-1-NT/azuredeploy.json
"resources": [
        {
            "apiVersion": "[variables('sfApiVersion')]",
            "type": "Microsoft.ServiceFabric/managedclusters",
            "name": "[parameters('clusterName')]",
            "location": "[resourcegroup().location]",
            "sku": {
                "name" : "[parameters('clusterSku')]"
            },
            "properties": {
                "dnsName": "[toLower(parameters('clusterName'))]",
                "adminUserName": "[parameters('adminUserName')]",
                "adminPassword": "[parameters('adminPassword')]",
                "clientConnectionPort": 19000,
                "httpGatewayConnectionPort": 19080,
                "clients" : [
                    {
                        "isAdmin" : true,
                        "thumbprint" : "[parameters('clientCertificateThumbprint')]"
                    }
                ],
                "loadBalancingRules": [
                    {
                        "frontendPort": 8080, 
                        "backendPort": 8080,
                        "protocol": "tcp",
                        "probeProtocol": "tcp"
                    }
                ]
            }
        },
        {
            "apiVersion": "[variables('sfApiVersion')]",
            "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
            "name": "[concat(parameters('clusterName'), '/', parameters('nodeTypeName'))]",
            "location": "[resourcegroup().location]",
            "dependsOn": [
              "[concat('Microsoft.ServiceFabric/managedclusters/', parameters('clusterName'))]"
            ],
            "properties": {
                "isPrimary": true,
                "vmImagePublisher": "[parameters('vmImagePublisher')]",
                "vmImageOffer": "[parameters('vmImageOffer')]",
                "vmImageSku": "[parameters('vmImageSku')]",
                "vmImageVersion": "[parameters('vmImageVersion')]",
                "vmSize": "[parameters('vmSize')]",
                "vmInstanceCount": "[parameters('vmInstanceCount')]",
                "dataDiskSizeGB": "[parameters('dataDiskSizeGB')]"
            }
        }
```

Microsoft.ServiceFabric/managedclusters と Microsoft.ServiceFabric/managedclusters/nodetypes のリソースをデプロイするだけのテンプレートとなっており非常にシンプルです。全体でも130行程度しかありません。   
なお、この2つのリソース定義については、こちらに[リファレンス](https://docs.microsoft.com/ja-jp/azure/templates/microsoft.servicefabric/managedclusters)があります。   
ちなみに nodetypes は、Service Fabric に関連付けられた仮想マシンのコレクション（VM Scaleset）を表します。なので OS Imageの種別や VM サイズやディスクサイズなど VM に関するパラメータを持っているんですね。
Microsoft.ServiceFabric/managedclusters では従来 NRP (Network Resource Provider) や CRP (Compute  Resource Provider)で定義されていたロードバランサーの設定や各ノードの管理者アカウント情報の設定が含まれているところが特徴的です。これらの設定を元にユーザが定義する必要なくロードバランサーや VM リソースが作成されていきます。

# リソース構成
[このテンプレート](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/SF-Managed-Basic-SKU-1-NT)を使って実際に Managed Cluster をデプロイしてみました。   
※デプロイ時には Service Fabric Explorer などに接続する際に利用するクライアント証明書が必要です。今回は検証用にCN=<デプロイするクラスタ名>.<リージョン>.cloudapp.azure.comの自己証明書を作成しておきthumprintをデプロイパラメータに指定しました。

デプロイしたリソースグループを見てみると、マネージド Service Fabric クラスターという見慣れない種類のリソースが作成されています。※keyvaultは証明書用に別途作成したリソースでテンプレートとは無関係
![image.png](/img/2021/Jan/ManagedServiceFabric/7f36c94a-d820-ddee-7e5b-066f0fe944e9.png)

この managedsf というリソースを選択してみましょう。...まだポータルからはデプロイ時に指定した細かな構成などは確認できず最低限の情報しか見れないようですね。
![image.png](/img/2021/Jan/ManagedServiceFabric/d97e7b2c-f640-be69-2293-a105c2bd9f14.png)

[Resource Explorer](https://resources.azure.com/)だと細かい構成値が見えるようです。ここらへんは一般提供時には拡充されているでしょう。
![image.png](/img/2021/Jan/ManagedServiceFabric/7b21cf5f-343b-fa39-2a1e-ace5f905a992.png)

次に Service Fabric Cluster の本体となるロードバランサーや VM Scaleset です。これまでの Service Fabric だと Service Fabric Clusterと同一のリソースグループにすべてのリソースが共存していましたが、Managed Cluster ではどうやらSFC_<ClusterID>のリソースグループ配下に配置されるようです。

![image.png](/img/2021/Jan/ManagedServiceFabric/377cefff-b163-cc96-dcc6-e4442a0e94cb.png)

配下のリソースなどを確認すると Microsoft.ServiceFabric/managedclusters と Microsoft.ServiceFabric/managedclusters/nodetypes で設定した値に沿って関連リソースが作成されていることがわかります。
![image.png](/img/2021/Jan/ManagedServiceFabric/46f3716b-6a76-8950-5e95-132b0b4ce608.png)


# おわりに
実際に Service Fabric Managed Cluster をデプロイしてみましたが、まだまだポータル操作などは制限が多いようですので実際にノードのスケール操作などを試すのであれば Powershell などコマンドラインからとなりそうです。
ノードのディスク構成周りも既存の Service Fabric と変わっているみたいなので、機会があればそちらもまとめてみたいと思います。

# 参考資料
[Azure Service Fabric managed clusters are now in public preview](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-managed-clusters-are-now-in-public-preview/ba-p/1721572)

[Azure Resource Manager テンプレートを使用して Service Fabric マネージド クラスター (プレビュー) をデプロイする](
https://docs.microsoft.com/ja-jp/azure/service-fabric/quickstart-managed-cluster-template)