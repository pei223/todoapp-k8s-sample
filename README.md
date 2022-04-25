# kubenetesでTODOアプリをクラスター内のDBに接続して動かしてみる
k8sの勉強用


## やりたいこと
- namespace
- HPA
- ingress
- kustomization


## apply/delete
```
kubectl apply -k todoapi
kubectl delete -k todoapi
```

## PVC削除
```
kubectl delete pvc db-pvc-postgresdb-0 
```

## Pod/Service表示
```
# 全部表示
kubectl get po
kubectl get svc

# label検索表示
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