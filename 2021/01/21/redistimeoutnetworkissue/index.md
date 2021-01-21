# Azure Redis Timeouts - Network Issues


先日の[Server Side Issues](../redistimeoutserversideissue/)に引き続き、[Azure Redis Timeouts - Network Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-network-issues/ba-p/2022222) も日本語翻訳してみました。
### 概要
クライアント、ネットワーク、サーバーサイドの要因など、Redis クライアント側でタイムアウトが発生する理由は多数あります。
また、使用するクライアントライブラリによってエラーメッセージが異なる場合もあります。
クライアントアプリケーションが Redis サーバー側からの応答をタイムリーに受信できない場合、Azure Cache for Redis のタイムアウトがクライアント側で発生します。
タイムアウト値が期限切れになるより前に、クライアントアプリケーションがレスポンスを受信しなかった場合は、いかなる場合もタイムアウトが発生し、Redisタイムアウトエラーメッセージとしてクライアントアプリケーションログに記録されます。   
ネットワークでは、connection failure（接続障害）、network blips(瞬断)、network failures（通信障害）、クライアントまたはサーバーサイドのネットワーク帯域幅制限の超過、その他の問題により、Redis タイムアウトが発生する可能性があります。   
以下は、一般的なネットワークの要因によりタイムアウトが発生する一般的な要因です。

1. Network blips
2. Client Network bandwidth limit exceeded（クライアントサイドのネットワーク帯域制限の超過）
3. Server Network bandwidth limit exceeded（サーバーサイドのネットワーク帯域制限の超過）

クライアント、またはサーバー起因の Redis タイムアウトの原因については、次の TechCommunity の記事を参照してください 。<br>
[Azure Redis Timeouts - Client Side Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-client-side-issues/ba-p/2022203)<br>
[Azure Redis Timeouts - Server Side Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-server-side-issues/ba-p/2022202)

***Microsoftの推奨事項：*** <br>
Microsoft は、Redis クライアントアプリケーションと同じ Azure リージョンで Azure Cache for Redis を使用することを推奨しています。
その主な理由は、各 Azure リージョンのバックボーンネットワークで使用されている巨大な帯域幅を低レイテンシで利用することができるので、より小さい帯域幅のパブリックネットワークの使用を回避できることです。Azure ネットワークバックボーンには、あらゆる障害に対する回復力を高めるためにさまざまな手段を講じています。 各 Azure リージョンのネットワークバックボーンで最終的に障害が発生すると、Azure Cache for Redis の接続だけでなく、すべての Azure サービスとすべての顧客が影響を受けます。これがクライアントアプリケーションとは異なる Azure リージョンで　Azure Cache for Redis サービスを使用しないようにする主な理由です。

### 1- Network blips
Network blips は、Azure Load Balancer の操作、Redis または ホストのアップデート/パッチによるフェイルオーバー、またはその他の理由によって発生する可能性があります。
Network blips は一過性であるため、常にクライアント側で発生した Network blips の原因を特定できるとは限りません。 
Network blips 自体は想定されたことであるため、Microsoft は、これらの一時的な Network blips に対処するために、クライアント側アプリケーションで再試行ポリシーを使用することをお勧めします。
これにより、クライアントアプリケーションの再試行によって Network blips から回復し、エンドユーザーがダウンタイムに遭遇することを回避することができます。

StackExchange.Redis クライアントライブラリには、このような接続エラーの処理方法を制御する AbortConnect という名前の設定があります。この設定のデフォルト値は「True」です。これは状況によっては自動的に再接続されないことを意味します。Microsoft の推奨事項は、AbortConnect を false に設定して、ConnectionMultiplexer が自動的に再接続できるようにすることです。

### 2- Client Network bandwidth limit exceeded
Redis タイムアウトの理由がクライアントネットワークの帯域幅制限の超過によるものかを特定するには、Redis クライアントアプリケーションで使用されるクライアント環境（AppService、VM、VMSS、AKSなど）上のネットワーク使用状況を調査して、ネットワークインターフェイスの制限やネットワーク帯域幅の制限（インバウンドまたはアウトバウンド）に達しているかどうかを確認する必要があります。
Redis クライアントアプリケーションだけでなく、クライアント環境のすべてのアプリケーションがネットワークの使用率の上昇に影響する可能性があることに注意してください。また、Redis タイムアウトは、ネットワーク帯域幅の使用率が100％の場合のみに引き起こされるわけではなく、値が100％に近くづくほど、ネットワークレイテンシの上昇や Redis タイムアウトの発生する可能性が高くなります。
これをトラブルシューティングするには、クライアント（AppService、VM、VMSS、AKSなど）に応じて、クライアントに適したネットワークツールが必要になります。

### 3- Server Network bandwidth limit exceeded
Redis タイムアウトの理由がサーバーネットワークの帯域幅制限の超過によるものかを特定するために、Azure ポータルには Redis もしくは Azure Monitor ブレード上に 2 つの重要なメトリックがあります。「Cache Read」と「Cache Write」です。これらは、指定されたレポート間隔に、キャッシュとの間で読み書きされたデータの量をメガバイト/秒（MB/s）で示します:[使用可能なメトリックとレポート期間](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-how-to-monitor#available-metrics-and-reporting-intervals)    
Redisクラスター化インスタンスの場合、「Cache Read」と「Cache Write」メトリックは、Redis インスタンス、またはシャードごとに使用できます。 ネットワーク帯域幅制限を超過したシャードを特定できるように、各シャードのネットワークの使用状況の調査が必要な場合があります。
このメトリックを使用して、使用中の Redis Tier の Azure Redis cache ネットワークパフォーマンス上限値と比較できます。[Azure Cache for Redis のパフォーマンス](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-planning-faq#azure-cache-for-redis-performance)

![ReadWrite.png](/img/2021/Jan/RedisNetworkTimeout/ReadWrite.png)

その他のネットワークの原因は、問題の発生原因がどこにあるかをより詳しく把握するために、ネットワークトレースを使用してクライアントサイドを調査する必要があります。
クライアントでのネットワークパケットトレースは、ネットワークチームが問題の原因を特定するのに役立つ場合があります。
また、特にネットワーク接続に関するトラブルシューティングが必要な場合は、[Azure Redis接続の問題のトラブルシューティング方法](https://techcommunity.microsoft.com/t5/azure-paas-blog/troubleshooting-azure-redis-connectivity-issues/ba-p/1450361) （Tech Communityの記事）を参照してください。 

### おわりに
クライアント、サーバー、またはネットワーク側でなんらかの問題が発生すると Azure Cache for Redis への接続でタイムアウトが発生する可能性があります。
クライアント、またはサーバーサイド起因の Redis タイムアウトの原因については、次の TechCommunity の記事を参照してください 。

[Azure Redis Timeouts - Client Side Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-client-side-issues/ba-p/2022203)<br>
[Azure Redis Timeouts - Server Side Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-server-side-issues/ba-p/2022202)

### 関連ドキュメント
[Timeout issues](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-troubleshoot-timeouts)<br>
[Redis Client handling - TCP keepalive](https://redis.io/topics/clients#tcp-keepalive)<br>
[Best practices for Azure Cache for Redis (general and client library specific)](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-best-practices)<br>
[StackExchange.Redis best practices](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-stackexchange-redis-md)<br>
[Available metrics and reporting intervals](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-how-to-monitor#available-metrics-and-reporting-intervals)<br>
[Azure Cache for Redis performance](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-planning-faq#azure-cache-for-redis-performance)<br>
[Troubleshooting Azure Redis Connectivity Issues (Tech Community article)](https://techcommunity.microsoft.com/t5/azure-paas-blog/troubleshooting-azure-redis-connectivity-issues/ba-p/1450361)<br>