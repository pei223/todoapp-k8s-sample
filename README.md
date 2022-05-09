# kubenetesでTODOアプリをクラスター内のDBに接続して動かしてみる
k8sの勉強用

## やりたいこと
- namespace
- HPA
- kustomization
- 簡易的にSSL/TLS
- rback
- network policy

## 環境
Docker Destktop kubernetesを使用

単一ノード構成


## Setup
ingress controllerをインストール
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.47.0/deploy/static/provider/cloud/deploy.yaml
```

hostsに以下の記述を追加
```
127.0.0.1 example.com
```

## apply

```
kubectl apply -k todoapi

# データ追加
curl -d '{"title":"sample project1"}' -H "Content-Type: application/json" -X POST http://localhost:3333/projects

# 以下のURLにアクセス
example.com/projects
```

## delete
```
kubectl delete -k todoapi
```

## PVC削除
```
kubectl delete pvc db-pvc-postgresdb-0 
```

## Pod/Service表示
```
# pods
kubectl get po
# service
kubectl get svc
# persistent volume claim
kubectl get pvc
# statefulset
kubectl get sts

# -wで監視
kubectl get po -w

# label検索
kubectl get po -l app=<labels/>appの値>
kubectl get svc -l app=<labels/>appの値>
```

## log
```
kubectl get po
kubectl logs <pod id>
```

## Pod/Service詳細
```
kubectl describe po <pod名>
kubectl describe svc <service名>
kubectl describe sts <statefulset名>
kubectl describe pvc <PersistentVolumeClaim名>
```


## 使ってるdocker image
- https://hub.docker.com/r/endofcake/go-todo-rest-api-example
- https://github.com/endofcake/go-todo-rest-api-example

## クラスター内へのDB接続はどうやるか
- hostはService名で解決できる
  - metadata/nameの値


## labelsの使い道
- apply/deleteで-lオプション使って一部だけ適用などできる
- get poなどで-lでラベル使って検索できる


## パブリッククラウドに対応するには
- ingress controller周り
- StatefulSetのDBは不要、RDSとかにする
- 