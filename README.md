# WebOTX Operator for Kubernetes

WebOTX Operator for Kubernetes は、コンテナオーケストレーションツールであるKubernetes上に WebOTX Application Serverのコンテナイメージを配備・稼働させるための自動化製品です。

## 動作環境

|ソフトウェア|動作バージョン|
|---|---|
|Kubernetes|1.14～1.18|
|OpenShift|4.2, 4.4, 4.5|

## クイックスタート

### Operator 関連のリソースをインストール

**Namespace の作成**  
必要に応じて WebOTX Operator for Kubernetes が動作する Namespace を作成してください。
Namespace を作成しない場合は、default に WebOTX Operator for Kubernetes が展開されます。
```
$ kubectl create namespace <Namespace名>
```

**カスタムリソース定義(CRD)の登録**  
Kubernetes にカスタムリソース定義(CRD)を登録します。
```
$ kubectl apply -f manifest/webotx_crd.yaml
```

**注意:** カスタムリソース定義は、全ての Namespace に適用されます。
このため、Kubernetes への登録は 1 回のみ適用します。

**サービスアカウント・権限の登録**  
WebOTX Operator コンテナを起動する為のサービスアカウントおよび権限を登録します。  
```
$ kubectl apply -f manifest/service_account.yaml -n <Namespace名>
```

**ConfigMap の登録**  
WebOTX Application Server コンテナの起動に必要な ConfigMap を登録します。  
```
$ kubectl apply -f manifest/configmap.yaml -n <Namespace名>
```

**manifest/operator.yaml の編集**  
manifest/operator.yaml ファイル内の下記の編集項目を修正します。

| 編集項目                        | 内容                              |
|--------------------------------|-----------------------------------|
| REPLACE_WEBOTX_OPERATOR_IMAGE  | WebOTX Operator コンテナイメージ名  |
| REPLACE_LICENSE_KEY            | WebOTX Operator ライセンスキー     |

**WebOTX Operator の展開**  
Kubernetes 上に WebOTX Operator を展開します。
展開が完了するまで時間が掛かります。
```
$ kubectl apply -f manifest/operator.yaml -n <Namespace名>
```

**WebOTX Operator の確認**  
WebOTX Operator が展開されていることを確認します。
```
$ kubectl get pod -n <Namespace名>
```

以下のような結果が表示されます。STATUS が Running 、Ready が 1/1 になれば正常に展開されています。
```
NAME                               READY   STATUS    RESTARTS   AGE
webotx-operator-684c7976cc-vxw4j   1/1     Running   0          14s
```

### WebOTX Application Server のインストール

**manifest/webotx_cr.yaml の編集**  
manifest/webotx_cr.yaml ファイル内の下記の編集項目を修正します。

| 編集項目　                      | 内容　　　                                    |
|--------------------------------|----------------------------------------------|
| REPLACE_APPLICATIONSERVER_NAME | 任意のカスタムリソース名                       |
| REPLACE_WEBOTX_AS_IMAGE        | WebOTX Application Server コンテナイメージ名   |

**カスタムリソースの登録**  
カスタムリソースを登録して、Kubernetes 上に WebOTX Application Server を展開します。
展開が完了するまで時間が掛かります。
```
kubectl apply -f manifest/webotx_cr.yaml -n <Namespace名>
```

**WebOTX Application Server の確認**  
WebOTX Application Server が展開されていることを確認します。
```
$ kubectl get pod -n <Namespace名>
```

以下のような結果が表示されます。STATUS が Running 、Ready が 2/2 になれば正常に展開されています。
```
NAME                               READY   STATUS    RESTARTS   AGE
webotx-as-6bf7569998-dvs9f         2/2     Running   0          97m
```

### WebOTX Application Server のアンインストール

**カスタムリソースの削除**  
登録しているカスタムリソースを削除します。
削除が完了するまで時間が掛かります。
```
kubectl delete ApplicationServer <カスタムリソース名> -n <Namespace名>
```

**WebOTX Application Server の確認**  
WebOTX Application Server が削除されていることを確認します。
```
$ kubectl get pod -n <Namespace名>
```

### WebOTX Operator のアンインストール

**WebOTX Operator の削除**  
展開している WebOTX Operator を削除します。
```
kubectl delete -f manifest/operator.yaml -n <Namespace名>
kubectl delete -f manifest/service_account.yaml -n <Namespace名>
```

**ConfigMap の削除**  
登録している ConfigMap を削除します。
```
kubectl delete -f manifest/configmap.yaml -n <Namespace名>
```

**カスタムリソース定義(CRD)の削除**  
登録しているカスタムリソース定義(CRD)を削除します。
```
kubectl delete -f manifest/webotx_crd.yaml
```

---

WebOTX製品に関するお問い合わせ  
https://jpn.nec.com/webotx/feedback.html  
ご購入前のお問い合わせ
