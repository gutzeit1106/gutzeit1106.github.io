# Microsoftのマイクロサービス構築フレームワーク Dapr を動かす


2019年10月にマイクロサービスアプリケーションの開発を容易にするためのフレームワークとして"Dapr"がマイクロソフトから公開されました。現在α版となっています。
Daprは、プログラミング言語に依存せずマイクロサービス間の呼び出し機能やステート管理、サービス間のメッセージ機能、リソースのバインディング、　分散サービス間のトレーシングなどの機能を提供するソフトウェアです。
マイクロサービスに直接組み込むのではなく、HTTP/gRPC API経由で呼び出して利用することで、アプリケーションに依存せず利用できることを目指しています。

<img src="/img/20191116_dapr/dapr_conceptual_model.jpg" align="left"><br clear="left">

ローカルでインストールし動作を試して見ました。

### 1. ドキュメント
Dapr 公式<br>
https://dapr.io/#

Dapr github<br>
https://github.com/dapr/docs

Roadmap<br>
https://github.com/dapr/dapr/wiki/Roadmap

0.2.0 Release Milestone<br>
https://github.com/orgs/dapr/projects/8

### 2. インストール(Mac)
Dapr CLIをインストールする
```sh
$ curl -fsSL https://raw.githubusercontent.com/dapr/cli/master/install/install.sh | /bin/bash
Your system is darwin_amd64
Installing Dapr CLI...

Downloading https://github.com/dapr/cli/releases/download/v0.1.0/dapr_darwin_amd64.tar.gz ...
dapr installed into /usr/local/bin successfully.
cli version: 0.1.0 
runtime version: n/a
To get started with Dapr, please visit https://github.com/dapr/docs/tree/master/getting-started
```
CLI を使って Dapr Runtime をインストールする。（事前にdockerを稼働させる）

```sh
$ dapr init
⌛  Making the jump to hyperspace...
✅  Downloading binaries and setting up components...
✅  Success! Dapr is up and running
```

>To see that Dapr has been installed successful, from a command prompt run the docker ps command and check that the daprio/dapr:latest and redis >container images are both running.

Dapr が正常にインストールされていていれば、daprio/dapr:latest と redis のコンテナイメージが見えるらしい

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                      NAMES
b5c379de74b6        daprio/dapr         "./placement"            59 seconds ago       Up 57 seconds       0.0.0.0:50005->50005/tcp   dapr_placement
44a1d7cb8aff        redis               "docker-entrypoint.s…"   About a minute ago   Up 58 seconds       0.0.0.0:6379->6379/tcp     epic_golick
```

### 3. サンプルコード （Hello World）

#### Hello World の解説
https://github.com/dapr/samples/tree/master/1.hello-world
<img src="/img/20191116_dapr/Architecture_Diagram.png" align="left"><br clear="left">
<br>

まずは　git clone

```
$ git clone https://github.com/dapr/samples.git
$ cd samples/1.hello-world
$ ls
README.md               app.js                  img                     package-lock.json       package.json            sample.http             sample.json
```

app.js を見ていく。<br>

```node
const stateUrl = `http://localhost:${daprPort}/v1.0/state`;
```

Dapr CLI は、 Dapr port用の環境変数を作成し、デフォルトでポートは 3500 。
次に neworder ハンドラを見ていく。

```node
app.post('/neworder', (req, res) => {
    const data = req.body.data;
    const orderId = data.orderId;
    console.log("Got a new order! Order ID: " + orderId);

    const state = [{
        key: "order",
        value: data
    }];

    fetch(stateUrl, {
        method: "POST",
        body: JSON.stringify(state),
        headers: {
            "Content-Type": "application/json"
        }
    }).then((response) => {
        console.log((response.ok) ? "Successfully persisted state" : "Failed to persist state");
    });

    res.status(200).send();
});
```
ここでは neworder メッセージを受信して処理するエンドポイントを公開している。
最初に受信したメッセージをコンソールログに記録して、次に stateUrl に state Key-Valueを POST することで　Redis にorder id を永続化する。


```node
app.get('/order', (_req, res) => {
    fetch(`${stateUrl}/order`)
        .then((response) => {
            return response.json();
        }).then((orders) => {
            res.send(orders);
        });
});
```
この Get エンドポイントでは、Redis キャッシュが呼び出され、order keyの最新 value が取得でき、Node.js app がステートレスになる。

#### Node.js アプリを Dapr と稼働させてみる
まずは依存関係をインストールする

```sh
$ npm install
```

Node.js app を稼働させる。

```sh
$ dapr run --app-id mynode --app-port 3000 --port 3500 node app.js
ℹ️  Starting Dapr with id mynode. HTTP Port: 3500. gRPC Port: 50880
✅  You're up and running! Both Dapr and your app logs will appear here.
```

フォアグラウンドで app が動くので別のターミナルなどで curl でエンドポイントに post する。
curl 実行は samples/1.hello-world に移動しておこなう。  

```
curl -XPOST -d @sample.json http://localhost:3500/v1.0/invoke/mynode/method/neworder
```
dapr app が稼働しているターミナルでメッセージの受信が確認できる。

```
== APP == Got a new order! Order ID: 42
== APP == Successfully persisted state
```

GET のエンドポイントにアクセスしてステートが正常に保存されていることを確認する。

```
$ curl http://localhost:3500/v1.0/invoke/mynode/method/order
{"orderId":"42"}
```

最後に次のコマンドでサービスを終了させる。

```
$ dapr stop --app-id mynode
✅  app stopped successfully
```

dapr の状態は dapr list で確認できる。

```
$ dapr list
No dapr instances found.
```
