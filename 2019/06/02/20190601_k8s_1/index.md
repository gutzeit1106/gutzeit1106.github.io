# k8s Cheat Sheet #1 -基本

・help参照 
```sh
kubectl --help 
```

・version確認(Client と Server ) 
```sh
kubectl version 
```

・yaml minifestの適用 
```sh
Kubectl -f FileName 
```
 
k8sクラスタのローカルcontext関連のコマンド 

・Local Configの一覧表示 
```sh
kubectl config view 
```

・現在アクティブになっているcontextの表示 
```sh
kubectl config current-context 
```

・アクティブなcontextの切り替え 
```sh
kubectl config use-context CONTEXT_NAME  
```

・Contextの一覧表示 
```sh
kubectl config get-contexts 
```

・Local Configのクラスタ名だけ一覧表示
```sh
kubectl config get-clusters
```