# Shell で Azure REST API を Call する


Azure には リソースの管理用の [REST API](https://docs.microsoft.com/ja-jp/rest/api/azure/) が用意されている。ポータルや Azure Powershell、CLI、Management SDKといったツール群は、この REST-API を Call して操作を実現している。  
大体の場合はポータルやツール群を使用することで事足りるけども、新機能の操作やREST-APIのリクエストを自由に指定して操作を試したい場合に REST を直接呼び出したい場合があり、シェルからさくっと呼び出したいという声をわりとよく聞く。
自分の知る限り多くの人は REST-API のヘッダに付加する Authrozation Token ( Azure に対する認証部分) が難しいと感じていると思うけど、わりとシンプルに curl で認証トークン取ってきて API を呼び出すことができるのでサンプルを作ってみた。

### 1. Azure AD アプリケーションとサービスプリンシパルの作成
非対話でシェルから認証しトークンを取得するには Azure AD アプリケーションの登録とサービスプリンシパルが必要になる。
Azure CLI を使用して、az ad sp create-for-rbac コマンドでサービスプリンシパルを作成する。

```sh
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<YourSubscriptionId>"
# {
#  "appId": "xxxxx",
#  "displayName": "xxxx",
#  "name": "http://azure-cli-xxx",
#  "password": "xxxxx",
#  "tenant": "xxxxx"
# }
```

### 2. アクセストークンの取得
1 のサービスプリンシパルを用いて curl でアクセストークンを取得する。

```sh
curl -X POST -d 'grant_type=client_credentials&client_id=[APP_ID]&client_secret=[PASSWORD]&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/[TENANT_ID]/oauth2/token
{"token_type":"Bearer","expires_in":"3599","ext_expires_in":"3599","expires_on":"1586163370","not_before":"1586159470","resource":"https://management.azure.com/","access_token":"xxxx"}
```

### 3. REST-API を CALL する
2 のレスポンスに含まれる access_token を リクエストのヘッダー（Authorization）に付加して、Azure REST API を Call する。以下はサブスクリプション内のリソース一覧を取得する例。

```sh
curl -X GET -H "Authorization: Bearer [TOKEN]" -H "Content-Type: application/json" https://management.azure.com/subscriptions/xxx/resources?api-version=2019-10-01
```

### 補足
アクセストークンや Azure REST API は JSON 形式で返るので parser として jq を入れておくと色々と便利だと思う。

```sh
apt-get -y install jq
```

整理するとこんな感じでシンプルに書ける。

```sh
APP_ID="xxxx"
PASSWORD="xxxx"
TENANT_ID="xxxx"
SUBSCRIPTION_ID="xxxx"
accessToken=`curl -X POST -d "grant_type=client_credentials&client_id=${APP_ID}&client_secret=${PASSWORD}&resource=https%3A%2F%2Fmanagement.azure.com%2F" https://login.microsoftonline.com/${TENANT_ID}/oauth2/token | jq -r .access_token`
curl -X GET -H "Authorization: Bearer ${accessToken}" -H "Content-Type: application/json" https://management.azure.com/subscriptions/${SUBSCRIPTION_ID}/resources?api-version=2019-10-01
```
