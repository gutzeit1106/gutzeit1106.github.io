# Azure Redis Timeouts - Client Side Issues


先日の [Server Side Issues](/2021/01/20/redistimeoutserversideissue/) と [Network Issues](/2021/01/21/redistimeoutnetworkissue/) に引き続き、 [Azure Redis Timeouts - Client Side Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-client-side-issues/ba-p/2022203) も日本語翻訳してみました。

### 概要
クライアント、ネットワーク、サーバーサイドの要因など、Redis クライアントでタイムアウトが発生する理由は多数あります。
また、使用するクライアントライブラリによってエラーメッセージが異なる場合もあります。 
クライアントアプリケーションが Redis サーバー側からの応答をタイムリーに受信できない場合、Azure Cache for Redis のタイムアウトがクライアント側で発生します。
タイムアウト値が期限切れになるより前に、クライアントアプリケーションがレスポンスを受信しなかった場合は、いかなる場合もタイムアウトが発生し、Redisタイムアウトエラーメッセージとしてクライアントアプリケーションログに記録されます。

Redis サービスはクライアントアプリケーションのメモリキャッシュとして機能するため、Redis の呼び出しのパフォーマンスは高く、応答時間は小さくなります。そのためクライアントアプリケーションで使用されるタイムアウト値は通常小さくなります。つまり、クライアントのストレスや過負荷は、他のアプリケーションより先に Redis サービスに影響を与え、Redis リクエストのレイテンシーが高くなり、それにより Redis タイムアウトが発生する可能性があります。
 
以下に、一般的なクライアント起因によりタイムアウトが発生する要因を列挙します。
1. トラフィックバースト（Traffic Burst） <br>
2. 大きな Key Value サイズ (Large Key Value Sizes)<br>
3. クライアント CPU 高負荷<br>
4. クライアントネットワークの帯域幅超過<br>
5. クライアントタイムアウト構成<br>
6. アイドル接続<br>
7. クライアントメモリの高使用率<br>

サーバーサイドまたはネットワーク起因の Redis タイムアウトの原因については、次の TechCommunity の記事を参照してください。

[Azure Redis Timeouts - Server Side Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-server-side-issues/ba-p/2022202)<br>
[Azure Redis Timeouts - Network Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-network-issues/ba-p/2022222)

### 1. Traffic Burst
Traffic Burst とクライアントサイドでの不十分なスレッド設定が組み合わさると、Redis サーバーから送信済みでクライアントサイドではまだ消費されていないデータの処理が遅延する可能性があります。
これによってクライアントで Redis タイムアウトが発生する可能性があります。
クライアントライブラリにより様々な方法でこの問題を対処できます。

***Stackexchange.Redisの traffic Burst と ThreadPool 設定：*** <br>
Traffic Burst を処理し、ThreadPool を管理するために、Stackexchange.Redis クライアントライブラリには 2 種類のスレッドがあります。

- ***Worker***スレッド：Task.Run（…）またはThreadPool.QueueUserWorkItem（…）メソッドのコマンドを処理するために使用されます。
- ***IOCP***（I/O Completion Port）スレッド：ネットワークからの読み取りなど非同期IOが発生したときに使用されます。

各タイプではスレッドの最小数を定義しており、デフォルトでは、クライアントマシンのコア数が最小値として設定されます。
ThreadPool は、最小値に達するまで要求に応じて新しいスレッドを提供します。
IOCP または Worker が使用するスレッドの数（Busy）が最小（Min）に達すると、新しいスレッドの生成は調整（スロットル）されます。
スロットルが発生すると、新しいスレッドが追加される速度は 500 ミリ秒あたり 1 スレッドです。
クライアントアプリケーションが最小（Min）よりも多くのスレッドを必要とするトラフィックのバーストに陥った場合、幾つかのスレッドは調整され、Redis クライアントでタイムアウトが発生する可能性があります。

Stackexchange.Redis の例外には、次のような最小スレッドとBusyスレッドの数に関する有用な情報が含まれています。

---
Timeout performing EVAL, inst: 187, mgr: Inactive, err: never, queue: 74, qu: 1, qs: 73, qc: 0, wr: 0, wq: 0, in: 1536, ar: 0, IOCP: (Busy=1,Free=999,Min=2,Max=1000), ***WORKER***: (<font color="Red">Busy=40</font>,Free=32687,<font color="Red">Min=2</font>,Max=32767), clientName: RD123456789ABC

---

上記の例では、Busy 状態の Worker スレッドの数は 40 で、最小値は 2 に設定されています。
したがって、`38スレッド * 500ミリ秒 = 19秒`。40番目のスレッドは、作成されるまで少なくとも19秒待機した可能性があります。そのため、Redis がスレッドの空きを待つ時間が長すぎたためにこの例外をスローしたと考えられます。

各アプリケーションは、使用量と負荷に基づいて最小（Min）値を調整する必要がある場合があります。

***クライアント側からトラフィックバーストを監視する方法：*** <br>
- ネットワークレベルでは、ネットワーク使用量の予期しないスパイクという形で、Traffic Burst を任意のネットワークトラフィックアナライザーから観測できるかもしれません。
- Stackexchange.Redis クライアントライブラリログを使用して、上記のように、IOCP または ワーカースレッドプールで「Min」よりも高い「Busy」値を調査できます。
- ThreadPoolLogger アプリケーションを使用して、トラフィックのバーストの「ビジー」に基づいて「最小」値を監視および調整できます。 https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs

***Stackexchange.Redisで スレッドプールの Min 設定を調整する方法：*** <br>
Stackexchange.Redis でスレッドプール設定を構成するには、2つの異なる方法があります。

***1. Redis StackExchange（C＃）構成（クライアント）で SetMinThreads()を使用する：*** <br>
Stackexchange.Redis のスレッドプール設定は、Redis StackExchange(C#) の SetMinThreads() を使用してクライアントアプリケーションで構成できます。

```cs
private readonly int minWorkerThreads = 200;
private readonly int minIOThreads = 200;
void Application_Start(object sender, EventArgs e)
{
    // Code that runs on application startup
   (…)
   ThreadPool.SetMinThreads(minWorkerThreads, minIOThreads);
}
```
Note： SetMinThreads を呼び出すときに指定された値は、コアごとの設定では***ありません***。たとえば、4コアのマシンがあり、実行時に minWorkerThreads と minIoThreads を CPU あたり 200 に設定する場合は、ThreadPool.SetMinThreads（200, 200）を使用する必要があります。

***2. Microsoft Internet Information Services (IIS）Webサーバーで \<processModel\> を使用する：*** <br>

Machine.config の\<processModel\>構成要素（通常は %SystemRoot%\Microsoft.NET\Framework\[versionNumber]\CONFIG\）内にある minIoThreads または minWorkerThreads 構成設定を使用して、最小スレッド設定を指定することもできます。

```
<processModel> on Machine.config
```

この方法で最小スレッド数を設定することは、システム全体の設定であるため、通常はお勧めしません。<br>
Note：この構成要素で指定されている値は、コアごとの設定です。たとえば、4コアのマシンがあり、実行時に minIoThreads 設定を 200 にしたい場合は、`<processModel minIoThreads="50" />`を使用します。

### 2. 大きな Key Value サイズ (Large Key Value Sizes)
Redis キャッシュは小さいキーサイズ（通常は 1KB ）に最適化されており、より大きなキー値サイズを使用すると Redis タイムアウトが発生する可能性があります。

同時に行うリクエストが少ない場合は値のサイズを大きくしても問題ありませんが、短いスパンにキーの値のサイズが大きいリクエストが多数あると、 Redis でタイムアウトが発生する可能性があります。
Redis は設計上シングルスレッドシステムであるため、大きなキー値を処理すると、その後に続く他のすべてのリクエストがブロックされます。最初の大きなキー値に想定よりも時間がかかる場合、次のリクエストはタイムアウトします。 

次の例では、リクエスト「A」と「B」がサーバーに迅速に送信されます。サーバーは応答「A」と「B」の送信を迅速に開始します。データ転送時間によって、サーバーが迅速に応答する場合でも、応答「B」は応答「A」がタイムアウトするまで待機する必要があります。

![example.png](/img/2021/Jan/RedisClientTimeout/example.png)

これは、このドキュメントで説明されています: [要求または応答のサイズが大きい](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-troubleshoot-client#large-request-or-response-size)

Microsoft の推奨事項は、クライアントアプリケーションを最適化して、大きな値を持つ少数の要求ではなく、多数の小さな値を使用することです。
要約すると、大きな値のサイズは避け、小さいキーの値のサイズに分割して、より多くのリクエストを行う必要があります。

***クライアント側から、使用されている大きなキー値サイズを確認する方法：*** <br>
redis-cli.exe は、クライアントとして Azure Cache for Redis と対話するための一般的なコマンドラインツールであり、使用されている大きなキー値のサイズを調査するために使用できます。

このツールは Redis-io ディストリビューションの一部であり、Azure Cache for Redis で使用することもできます。 

redis-cli.exe コマンドラインツールの唯一の注意点は TLS をサポートしておらず、Azure Cache forRedis はデフォルトで TLS ポート(6380)のみを有効にします。 
このツールを使用するには、  SSL トンネルを使用して SSL/TLS アクセスを使用するか、非TLSポート(6379)を一時的に有効にします(アクセスキーは TCP 経由でクリアテキストで送信されるため、セキュリティ上の理由からお勧めしません)。
SSLトンネルを使用するには、Stunnel ツールを使用できます。このドキュメントが役立つでしょう。[redis-cli.exe に対するアクセスを有効化する](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-how-to-redis-cli-tool#enable-access-for-redis-cliexe)

 

最新の Redis.io ディストリビューションの Windows zip ファイルから redis-cli.exe コマンドラインツールをダウンロードして抽出します。[Redis-x64-3.0.504.zip](https://github.com/microsoftarchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.zip)から入手できます

![redis-cli.png](/img/2021/Jan/RedisClientTimeout/redis-cli.png)

次の構文で redis-cli を実行できます（非SSL接続の場合は-p 6379、SSL接続の場合は-p 6380）。
```
redis-cli -h <RedisName>.redis.cache.windows.net -p 6379 -a <RedisAccessPassword> --bigkeys
```

この特別なモードでは、redis-cli はキースペースアナライザーとして機能します。データセットをスキャンして大きなキーを探しますが、データセットを構成するデータ型に関する情報も提供します。
このモードは `--bigkeys` オプションで有効になり、非常に詳細な出力を生成します。]

```
# Scanning the entire keyspace to find biggest keys as well as
# average sizes per key type.  You can use -i 0.1 to sleep 0.1 sec
# per 100 SCAN commands (not usually needed).

[00.00%] Biggest string found so far 'key-419' with 3 bytes
[05.14%] Biggest list   found so far 'mylist' with 100004 items
[35.77%] Biggest string found so far 'counter:__rand_int__' with 6 bytes
[73.91%] Biggest hash   found so far 'myobject' with 3 fields

-------- summary -------

Sampled 506 keys in the keyspace!
Total key length in bytes is 3452 (avg len 6.82)

Biggest string found 'counter:__rand_int__' has 6 bytes
Biggest   list found 'mylist' has 100004 items
Biggest   hash found 'myobject' has 3 fields

504 strings with 1403 bytes (99.60% of keys, avg size 2.78)
1 lists with 100004 items (00.20% of keys, avg size 100004.00)
0 sets with 0 members (00.00% of keys, avg size 0.00)
1 hashs with 3 fields (00.20% of keys, avg size 3.00)
0 zsets with 0 members (00.00% of keys, avg size 0.00
```

redic-cliおよび--bigkeysオプションの詳細については、こちらを参照してください： [redis-cli, the Redis command line interface](https://redis.io/topics/rediscli)

***大きなキー値サイズの使用を回避する方法：*** <br>
大きなキー値サイズの使用を回避するために、Microsoft の推奨事項は、少数​​の大きな値ではなく、多数の小さな値に対してクライアントアプリケーションを最適化することです。
大きな値のサイズは避け、小さいキーの値のサイズに分割して、より多くのリクエストを行う必要があります。

### 3. クライアント CPU 高負荷（High Client CPU Load）
クライアントの CPU 使用率が高い場合、実行を要求された作業にシステムが追いつけない状況に陥る可能性があります。
もし CPU がビジー状態であれば、要求している作業にアプリケーションが追いつけず問題を引き起こす可能性があります。
Microsoft の推奨値は、クライアントの CPU が 80% を超えていないことです。

***クライアントの CPU 高負荷を確認する方法:*** <br>
- Azure ポータルからクライアント環境 (AppService、VM、AKS など)のブレードで使用可能なメトリックを使用して、クライアントのシステム全体の CPU 使用率を監視します。
- クライアントマシンのパフォーマンスカウンタを監視します。

***クライアントの CPU 使用率を制限する方法:*** <br>
- クライアントサイドで CPU 高騰、もしくは CPU スパイクを引き起こしている箇所を調査し、コードロジックを変更します。
クライアント CPU は通常、Redis クライアントアプリケーションだけでなく、(WebApps の場合) 同じ AppService Plan 内の他のアプリケーションや、VM、VMSS、AKSなどでは、他のアプリケーションやオペレーティングシステムのタスクなどで使用されます。
- CPU 容量が大きい、より大きな VM サイズにクライアントをアップグレードします。

### 4. クライアントネットワークの帯域幅超過（Client Network Bandwidth Exceeded）
クライアントネットワーク帯域幅の超過と、Redis タイムアウトの他のネットワーク要因については、この Tech Communitiy の記事で説明しています: [Azure Redis Timeouts - Network Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-network-issues/ba-p/2022222)

### 5. クライアントタイムアウト構成（Client Timeout Configurations）
各 Redis クライアントライブラリには、独自の構成オプションと、さまざまなタイムアウト値を構成する機能があります。各オプションとそれぞれのデフォルト値は、使用するクライアントライブラリによって異なる可能性があります。
例として、StackExchange.Redis では、タイムアウトをスローする可能性がある以下の構成オプションが使用されています。

| 構成文字列 | デフォルト値 | 説明 |
| -------- | ---------- | ---- |
| SyncTimeout | 5秒 | 特定の同期操作が完了するまでの待機時間 | 
| ResponseTimeout | SyncTimeout | 非推奨 - コネクションが切断されたものとして扱われるまでの時間 - 下位互換性のために使用 |
| ConnectTimeout | 5秒 | コネクションの試行に使用される時間（15秒を推奨） |
| ConnectRetry | 3 | 最初の接続から接続の試行を繰り返す回数 |
| keepAlive | 60秒 | ソケットを継続させるのに役立つメッセージを送信する時間 - 以下のアイドル接続を参照してください |


***Redis クライアント構成を確認する方法：*** <br>
Redis クライアント構成を確認するには、クライアントアプリケーションコードで接続文字列または構成オブジェクトを探し、使用されている値がケースシナリオとワークロードに最適かどうかを確認する必要があります。
Microsoft の推奨事項は、1 つの接続のみを使用し、接続を再利用することです。
Microsoft の推奨事項に従っていない場合、クライアントアプリケーションは、コードの様々な箇所で、異なる構成値に接続文字列を持っている可能性があることに注意してください。 

***Redis クライアントのタイムアウト値を設定する方法：*** <br>
***StackExchange.Redis クライアントライブラリ：***　<br>
Azure Redisへの接続は、接続文字列を介して行うことができます。Redis を構成する方法はたくさんあるため、StackExchange.Redis は、Connect を呼び出すときに使用できる豊富な構成モデルを提供します。
Stackexchange.Redis の ConnectTimeout 値（コネクションの試行に使用される時間）について、高い負荷状態でもシステムに接続する余裕を与えるため、Microsoftでは15000ミリ秒を推奨していますが、デフォルト値は5000ミリ秒です。
この値は調整することをお勧めします。

構成は次のいずれかになります。   
- ConfigurationOptions インスタンス：

```cs
string configString = "redis236.redis.cache.windows.net, abortConnect=false, ssl=true, password=XXX";
var options = ConfigurationOptions.Parse(configString);
options.AllowAdmin = true;
options.abortConnect=false;
options.connectTimeout=15000;
```

- 構成を表す文字列：
```cs
string configString = "redis236.redis.cache.windows.net, abortConnect=false, ssl=true, password=XXX, connectTimeout=15000";
```

両方の構成方法の切り替えは簡単に行うことができます。

```cs
ConfigurationOptions options = ConfigurationOptions.Parse(configString);
```
または、

```cs
string configString = options.ToString();
```

次に、configString または ConfigurationOptions インスタンスを使用して、新しい構成値で接続を作成します。

```cs
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(configString);
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options);
```

***その他のクライアントライブラリ固有のガイダンス：*** <br>
- [StackExchange.Redis (.NET)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-stackexchange-redis-md)
- [Java - Which client should I use?](https://gist.github.com/warrenzhu25/1beb02a09b6afd41dff2c27c53918ce7#file-azure-redis-java-best-practices-md)
- [Lettuce (Java)](https://gist.github.com/warrenzhu25/181ccac7fa70411f7eb72aff23aa8a6a#file-azure-redis-lettuce-best-practices-md)
- [Jedis (Java)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-java-jedis-md)
- [Node.js](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-node-js-md)
- [PHP](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-php-md)
- [ASP.NET Session State Provider](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-session-state-provider-md)

### 6. アイドル接続（Idle Connections）
Azure Cache for Redis は、使用するクライアントライブラリとは無関係に、接続に対して 10 分間のアイドルサーバータイムアウトを構成しています。
これはサーバー側で定義されており変更できません。
この場合、接続が 10 分間アイドル状態であれば、接続はリセットされ、使用しているクライアントライブラリによっては、ソケットリセットエラー、タイムアウト、
またはその他のコネクションエラーメッセージがクライアントに表示される場合があります。

StackExchange.Redis のような一部のクライアントは、Redis に対して定期的に ping 呼び出しを行い、接続を維持します (上記の keepAlive 構成を参照)。
一部のクライアントでは、これを行わないため、アプリケーション開発者は、この定期的な ping 呼び出しを実装して、アイドル接続の切断を回避する必要があります。
これについては、[Azure Cache for Redis のベストプラクティス](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-best-practices) に関するドキュメントで説明しています。

***アイドル接続を確認する方法：*** <br>
10 分間 Redis リクエストのないアイドル状態のコネクションがあり、ソケットリセットエラー、タイムアウト、またはその他のコネクションエラーメッセージが表示される場合、おそらく原因は keep alive ポリシーが実装されていないことです。
Stackexchange.Redis で説明されているように、keep alive　構成オプションがあるかどうか、使用しているクライアントライブラリを確認してみてください。

***アイドル接続を軽減する方法:***　<br>
keep alive 構成オプションを 60 秒 (Stackexchange.Redis の既定) に調整するか、またはクライアントライブラリごとに対応した構成値を指定します。
クライアントライブラリによっては、Connation をアクティブに維持するために、Redis エンドポイントへの定期的な ping を実行する独自の keep alive プロセスを実装する必要がある場合があります。
### 7. クライアントメモリの高使用率（High Client Memory Usage）
クライアントマシンのメモリ負荷は、cache からの応答処理を遅延させ様々なパフォーマンス問題を引き起こす可能性があります。
これが起こるとシステムはデータをディスクにページングし、ページフォールトが原因でシステムの速度が大幅に低下します。
これは Redis の呼び出しはパフォーマンスが高く、応答時間が低いと期待されているため、Redis の呼び出しに対して大きな影響を与えてしまう可能性があります。
そのような理由から、クライアントサイドでメモリ不足が発生すると、クライアント環境で実行される他のサービスよりも先に Redis タイムアウトが発生する場合があります。

***クライアントメモリの使用率が高いことを確認する方法:*** <br>
クライアントメモリ使用量の高騰は、Redis タイムアウトのタイムスタンプで確認することができ、クライアントマシンのメモリ使用量を監視して、利用可能なメモリ量を超過しないことを確認1してください。
また、メモリ負荷が原因でページフォールトが発生する可能性がありますので、クライアントの Page Faults/Sec パフォーマンスカウンターを監視することも役立ちます。

***クライアントのメモリ使用量を制限する方法:*** <br>
同時に実行中のアプリケーション/プロセスの数を減らしたり、多くのメモリを使用しているプロセスを調査したりすることで、クライアントのメモリ使用量を減らすことができます。
一部のプロセスが想定よりも大量のメモリを使用している場合は、このプロセス/アプリケーションをトラブルシューティングして、メモリ使用の原因を特定する必要があります。
もし全てが想定通りなのであれば、もっと多くのメモリリソースを使用できるようにクライアント環境をスケーリングする必要があります。

### おわりに
クライアント、サーバー、またはネットワーク側でなんらかの問題が発生すると Azure Cache for Redis への接続でタイムアウトが発生する可能性があります。
サーバーサイドまたはネットワーク起因の Redis タイムアウトの原因については、次の TechCommunity の記事を参照してください 。

[Azure Redis Timeouts - Server Side Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-server-side-issues/ba-p/2022202)<br>
[Azure Redis Timeouts - Network Issues](https://techcommunity.microsoft.com/t5/azure-paas-blog/azure-redis-timeouts-network-issues/ba-p/2022222)

### 関連ドキュメント
[Timeout issues](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-troubleshoot-timeouts)<br>

1- Traffic Burst<br>
[StackExchange.Redis timeout exceptions (GitHub article)  ](https://github.com/uglide/azure-content/blob/master/articles/redis-cache/cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions)<br>
[ThreadPool Growth: Some Important Details (Jon Cole article)](https://gist.github.com/JonCole/e65411214030f0d823cb)<br>
[Client-side issues](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-troubleshoot-client)<br>
[BusyIO or BusyWorker threads in the timeout exception (GitHub discussion)](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Timeouts.md#are-you-seeing-high-number-of-busyio-or-busyworker-threads-in-the-timeout-exception)<br>
[ThreadPoolLogger code sample (from Jon Cole)](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs)<br>
[Traffic-burst](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-troubleshoot-client#traffic-burst)<br>
[Recommendation ThreadPool settings](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-faq#recommendation)<br>
[processModel Element](https://docs.microsoft.com/ja-jp/previous-versions/dotnet/netframework-4.0/7w2sway1(v=vs.100))<br>

2- Large Key Value Sizes<br>
[redis-cli, the Redis command line interface](https://redis.io/topics/rediscli)<br>
[Enable access for redis-cli.exe](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-how-to-redis-cli-tool#enable-access-for-redis-cliexe)<br>
[Redis-x64-3.0.504.zip](https://github.com/microsoftarchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.zip)<br>

3- High Client CPU Load<br>
[High Client CPU usage](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-troubleshoot-client#high-client-cpu-usage)<br>

5- Client Timeout Configurations<br>
[Redis Client handling - TCP keepalive](https://redis.io/topics/clients#tcp-keepalive)<br>
[Azure Redis Best Practices documentation](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-best-practices)<br>
[StackExchange.Redis configurations](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)<br>

6- Idle Connections<br>
[Azure Redis Best Practices documentation](https://docs.microsoft.com/ja-jp/azure/azure-cache-for-redis/cache-best-practices)<br>