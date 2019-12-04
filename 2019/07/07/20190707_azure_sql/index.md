# Azure SQL DatabaseとCosmosDBの要点整理(2019/7)


## Azure SQL Database サーバ
Azure SQL Databaseサーバは、各種機能の中央管理をおこなう論理的リソースで、以下のような管理機能を提供している。

- データベースアクセス（ログイン）

- Firewall Rule

- 監査規則

- 脅威検出ポリシー（SQLインジェクション、ブルートフォース攻撃など）

- 自動フェールオーバーグループ

Azure SQL Databaseサーバは、Database を作成するにあたって事前に存在している必要がある。

## Azure SQL Database
Azure SQL Databaseには、次の3つのデプロイオプションがある。

- 単一データベース
- エラスティックプール
- マネージドインスタンス

### 単一データベース
最も汎用性の高いオプション。プランのスケールアップやスケールインが可能で柔軟性が高い。  
Database Migration Service(DMS)を使用することで、オンプレミスSQLサーバーからの移行も可能。DACパッケージ(dacpac)を使って移行することもできるが、bacpacファイルのエクスポートやAzure SQL Databaseへのインポートも発生するためダウンタイムは長い。

### エラスティックプール
エラスティックプールを使用することで複数のデータベースで共有されるプールのリソースを確保（購入）でき個々のデータベースの使用期間が予測しづらい場合にリソースを効率的に使用できる。エラスティックプール内のデータベースは単一のAzure SQL Database サーバ上に存在し設定されているリソースを共有している。
そのためエラスティックプールは、テナント単位で個々のDatabaseを展開したい場合などに魅力的なプランとなる。また、単一データベースはエラスティックプールの内外に移動することができる。

### マネージドインスタンス
オンプレミスSQL Serverと互換性の高いデータベースが作成できるため、リフト&シフトのケースで最初に検討されるオプション。
仮想ネットワークにインタンスを展開できるので、プライベートIPアドレスを持つ。(VPNやExpressRouteの利用も選択肢にできる)  
既存のSQL Serverからの移行には、Azure Database Migration Service(DMS)を使用することで、オフラインおよびオンライン移行できる。
オフライン移行では、移行の開始からアプリケーションのダウンタイムが発生するが、オンライン移行では、増分的にマネージドインスタンスにプッシュするので最終的な切り替え時のみダウンタイムが発生する。ネイティブのRESTOREコマンドで移行することもできるが、バックアップファイルの取得、コピーなどで移行ダウンタイムは大きくなる。

### 仮想コアベースとDTU ベースの購入モデル
仮想コアベースは、ハードウェアの世代とハードウェアの物理特性 (コア数、メモリ、ストレージ サイズなど) を選択して利用するスタイル
DTU(Database Transaction Unit) は、SQL Database のデータベースの処理性能を表す単位。Microsoftがベンチマークテスト数値に基づく
CPU、メモリの使用率、およびディスクの読み書き負荷の組み合わせた測定値に基づいて算出している。このDTUに基づいて購入するモデル。
DTUベースの購入モデルは、現在マネージドインスタンスでは提供されていない。

<!--https://www.slideshare.net/ssuser12e741/15jssugazure-sql-database -->

***

## Azure Cosmos DB
### 概要
モダンアプリ向けの低レイテンシ、大規模なスケールを保証するために構築されたフルマネージドグローバル分散データベース
Cosmos DBのユースケースとして、世界中に分散されたアプリケーションや、瞬間的かつ弾力的なスケールが必要なワークロードなどに対応できる
（Cosmos DB では処理種類に応じて特定の処理待機時間を超える要求を失敗したリクエストとしてSLA対象にしている）
「グローバル分散」と「マルチモデルのデータベース」が最大の特徴で、Document、Graph、Key-Value、Column-Familyをサポートしている。
データモデルにはそれぞれ対応するAPIが存在している

| DataModel     |  API                        | 説明                                                  |
| ------------- | --------------------------- | ----------------------------------------------------- |
| Documents     | SQL API, MongoDB API        | JSON形式のドキュメントを保存できるデータモデル            |
| Graph         | Gremlin API                 | データとデータの繋がりをグラフ構造で保持するデータモデル   |
| Key-Value     | Table API, Etcd(Preview)    | スキーマレスなテーブル構造でデータを保持するデータモデル   |
| Column-Family | Cassandra API               | Apache Cassandraベースのデータモデル                    |

### 移行
[Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool)が用意されており、このツールを使ってDocumentとTableは移行が可能。

| API         | 移行                                                                                                                              |
| ----------  | ---------------------------------------------------------------------------------------------------------------------------------- | 
| SQL API     | Data Migration Toolで提供される任意のソース オプションを使用してデータをインポートできる                                                | 
| Table API   | Data Migration Toolか、Azcopyを使用してデータをインポートできる                                                                        | 
| MongoDB API | Data Migration Toolは現在対応していない。Azure Database Migration Service(DMS) を使ってローカルのMongoDBをCosmos DBに移行できる。[MongoDBの接続情報か、Azure Storage上のbsondダンプを使ってインポートできる](https://docs.microsoft.com/ja-jp/azure/dms/tutorial-mongodb-cosmos-db?toc=/azure/cosmos-db/toc.json)        | 
| Gremlin API | 現在移行のサポートなし                                                                                                               | 