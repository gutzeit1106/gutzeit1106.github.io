# Azure Kubernetes Service(AKS)で Dapr を動かす（Setup編）


先日、[stand-alone な環境](../../../../2019/11/16/20191116_dapr/)で dapr の動作を検証しました。
次は、Kubernetes cluster 上で dapr の動作を検証してみたいと思います。Minikube などのローカルな環境でも試すことができそうでしたが、折角なので Azure Kubernetes Service(AKS) で検証してみます。

### 0. ドキュメント
Installing Dapr on a Kubernetes cluster <br>
https://github.com/dapr/docs/blob/master/getting-started/environment-setup.md#installing-dapr-on-a-kubernetes-cluster

Set up an Azure Kubernetes Service cluster <br>
https://github.com/dapr/docs/blob/master/getting-started/cluster/setup-aks.md

### 1. Azure Kubernetes Service(AKS) Cluster の作成
前提条件として、次を作業端末にセットアップします。

・Azure CLI <br>
・kubectl <br>

はじめに dapr 検証環境用の AKS Cluster を [Azure CLI](https://docs.microsoft.com/ja-jp/cli/azure/install-azure-cli?view=azure-cli-latest) で作成します。kubectl　も前提として必要になるので、[クイック スタート:Azure CLI を使用して Azure Kubernetes Service クラスターをデプロイする](https://docs.microsoft.com/ja-jp/azure/aks/kubernetes-walkthrough)を見ると良いかと思います。

インストール後に次のコマンドを実行していきます。

```sh
# Azure への接続
az login

# サブスクリプションの選択
az account set -s <YourSubscriptionId>

# リソースグループの作成
az group create --name myAKSdapr --location japaneast

# AKS クラスタの作成（k8sは、1.13.x 以上を使用する必要があるので必要に応じて--kubernetes-versionで指定する）
az aks create --resource-group myAKSdapr \
    --name myAKSDaprCluster \
    --node-count 2 \
    --enable-addons http_application_routing
    --enable-rbac \
    --generate-ssh-keys

# クラスタの資格情報を取得する
az aks get-credentials --resource-group myAKSdapr --name myAKSDaprCluster
```

なお、[k8s のバージョン 1.6.x で helm 2.15.0 以下が動作しない](https://github.com/helm/helm/issues/6374#issuecomment-537185486)という問題があるようなので、この後 dapr を helm でインストールする際は、kubernetes のバージョンに注意した方が良いです。


一応、クラスタ作成後に kubectl get node で確認しておきます。

```
$ kubectl get node
NAME                                STATUS   ROLES   AGE     VERSION
aks-nodepool1-37284717-vmss000000   Ready    agent   5m39s   v1.13.12
aks-nodepool1-37284717-vmss000001   Ready    agent   5m51s   v1.13.12
```

### 2. Dapr CLI　のインストール
今回の作業端末は、Mac ではなく Visual Studio Code の Remote Deployment で接続した　Ubuntu 環境のため、Dapr CLI をインストールする。
ちなみに、Mac 用の　dapr CLI のインストールは、[前回](../../../../2019/11/16/20191116_dapr/)を参照してください。<br>
次のコマンドで最新の dapr cli が、/usr/local/bin にインストールされる。

```
wget -q https://raw.githubusercontent.com/dapr/cli/master/install/install.sh -O - | /bin/bash
Your system is linux_amd64
Installing Dapr CLI...

Downloading https://github.com/dapr/cli/releases/download/v0.2.0/dapr_linux_amd64.tar.gz ...
dapr installed into /usr/local/bin successfully.
cli version: 0.2.0 
runtime version: n/a
To get started with Dapr, please visit https://github.com/dapr/docs/tree/master/getting-started
```

### 3. AKS クラスタに dapr をインストールする
次のコマンドで、AKS クラスタに dapr をインストールする。

```
$ dapr init --kubernetes
⌛  Making the jump to hyperspace...
ℹ️  Note: this installation is recommended for testing purposes. For production environments, please use Helm 

✅  Deploying the Dapr Operator to your cluster...
✅  Success! Dapr has been installed. To verify, run 'kubectl get pods -w' in your terminal
```

上記の通りインストールが正常に完了した後に、kubectl get pods で pod を確認すると、dapr関連の pod (dapr-operator, dapr-placement, dapr-sidecar-injector) が表示される。

```
$ kubectl get pods -w
NAME                                     READY   STATUS    RESTARTS   AGE
dapr-operator-68f7dcb454-nv25m           1/1     Running   0          70s
dapr-placement-6d77d54dc6-5bsp4          1/1     Running   0          70s
dapr-sidecar-injector-86d6ccf956-5j56p   1/1     Running   0          70s
```


この時、この pod は、default の namespace に作成されるが、helm を使ってインストールすることで、任意の namespace 配下に作成ができる。
アンインストールしたいときは、次のコマンドを実行します。

```
dapr uninstall --kubernetes
```


先ほどの 3 つの pod が Terminating になっていくことがわかります。

```
kubectl get pods
NAME                                     READY   STATUS        RESTARTS   AGE
dapr-operator-68f7dcb454-nv25m           0/1     Terminating   0          22m
dapr-placement-6d77d54dc6-5bsp4          0/1     Terminating   0          22m
dapr-sidecar-injector-86d6ccf956-5j56p   0/1     Terminating   0          22m
```

