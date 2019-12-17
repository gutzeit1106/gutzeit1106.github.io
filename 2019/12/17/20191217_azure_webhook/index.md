# Azure の自動スケール操作を webhook で Microsoft Teams / Slack に通知する


### はじめに
Azure では、 Cloud Service や Webapp、VM Scaleset などの各種リソースで事前に設定した条件に応じて自動でインスタンス数を増減させる自動スケールの機能が用意されています。  
システムの CPU 負荷などに応じて自動でリソース体制をアジャストできるためとてもクラウドらしい機能かと思いますが、運用を考えると、自動スケールが動作した際に e-mail や Slack、Microsoft Teams などに自動通知したいなどの要望も自然と出てきます。
e-mail は比較的容易に以下の手順で、サブスクリプションの管理者や任意のメールアドレス宛に通知をすることが可能ですが、Slack や Microsoft Teams への通知は webhook の通知を使って一工夫する必要があります。  


  Azure Monitor で自動スケール操作を使用して電子メールと webhook アラート通知を送信する  
  https://docs.microsoft.com/ja-jp/azure/azure-monitor/platform/autoscale-webhook-email


実現方法は多種あるかと思いますが、一案として Azure Logic App を使った方法を記録しておきます。

### 概要
ざっくりとした流れは次の通りです。

1. Logic App を作成する
2. Logic App で webhook を受けて lack / Microsoft Teams に通知するフローを構築する
3. 上記の　Logic App のエンドポイントを自動スケールの webhook 通知に設定する


### 1. Logic App を作成する

[Azure Portal](https://portal.azure.com/)から Logic App を作成します。

![LogicAppCreate](/img/20191217_azure_webhook/1-1.png)

リソース名などのパラメータは任意に設定ください。

### 2. Logic App で webhook を受けて slack / Microsoft Teams に通知するフローを構築する
Logic Apps デザイナーから、”HTTP 要求の受信時” をトリガーとして選択します。

![Flow1](/img/20191217_azure_webhook/2-1.png)

”要求本文の JSON スキーマ”には、自動スケールの webhook リクエストから受ける body の json スキーマを設定します。
JSON スキーマを設定することで、以降のフローである slack / Microsoft Teams の処理に動的な値として自動スケールの webhook リクエストに含まれている動的な値を含めることができるようになります。実例としては次になります。

```json
{
    "type": "object",
    "properties": {
        "version": {
            "type": "string"
        },
        "status": {
            "type": "string"
        },
        "operation": {
            "type": "string"
        },
        "context": {
            "type": "object",
            "properties": {
                "timestamp": {
                    "type": "string"
                },
                "id": {
                    "type": "string"
                },
                "name": {
                    "type": "string"
                },
                "details": {
                    "type": "string"
                },
                "subscriptionId": {
                    "type": "string"
                },
                "resourceGroupName": {
                    "type": "string"
                },
                "resourceName": {
                    "type": "string"
                },
                "resourceType": {
                    "type": "string"
                },
                "resourceId": {
                    "type": "string"
                },
                "portalLink": {
                    "type": "string"
                },
                "oldCapacity": {
                    "type": "string"
                },
                "newCapacity": {
                    "type": "string"
                }
            }
        },
        "properties": {
            "type": "object",
            "properties": {
                "key1": {
                    "type": "string"
                },
                "key2": {
                    "type": "string"
                }
            }
        }
    }
}
```

次に”+ 新しいステップ”をクリックして、Slack あるいは Microsoft Teams のコネクタを接続します。


***Slack の場合***

![Flow2](/img/20191217_azure_webhook/2-2.png)

![Flow3](/img/20191217_azure_webhook/2-3.png)

***Microsoft Teams の場合***

![Flow2b](/img/20191217_azure_webhook/2-2b.png)

![Flow3b](/img/20191217_azure_webhook/2-3b.jpg)


上記を設定して、Logic Apps デザイナーを保存します。保存すると、”HTTP 要求の受信時” にエンドポイントの URL が生成されるのでメモしておきます。  
次のような URL が生成されます。

![Flow3b](/img/20191217_azure_webhook/2-4.png)

### 3. Logic App のエンドポイントを自動スケールの webhook 通知に設定する
”HTTP 要求の受信時” のエンドポイント URL を自動スケールの通知先として設定します。VM Scaleset の場合は、次の画面から設定することが可能です。

![autoscale](/img/20191217_azure_webhook/3-1.png)

### 4. 動作確認
これで設定は完了です。最後に動作確認をします。自動スケールを動作させるため既存のインスタンスの CPU 負荷を高くします。（今回は CPU 使用率で自動スケールする設定をしています）  
Linux 環境なので、ssh 接続し次のコマンドを実行しました。

```
yes >> /dev/null &
```

しばらく待って自動スケールが実行されると、次の通り設定通りの通知がきました。

***Slack の場合***
![result](/img/20191217_azure_webhook/4-1.png)  

***Microsoft Teams の場合***
![result2](/img/20191217_azure_webhook/4-1b.jpg)