# ArgoCD 

## 1. Repo連結要和ArgoCD結合, 可以透過kubectl指令
`kubectl apply -f ./argocd/repo-creds.yaml`

目標是所有的repo都可以吃到ArgoCD

## 2. 創建app

方法1: 在ui介面點選
方法2: 在base資料夾中複製`argocd/base/backend-deployment.yaml`和`argocd/base/kustomization.yaml`

```
🧱 白話說：kustomization是「拼積木的說明書」
你有很多散開的 K8s 資源檔案（像是 deployment.yaml、service.yaml、configmap.yaml）

而 kustomization.yaml 是用來告訴工具：

👷‍♂️「我這一組要組這些 YAML，而且我還想 patch 一點內容，比如 replica 數量要改成 2。」
```

## 3. 創建overlays/dev

```
你只要維護一套共用樣板（base），然後針對不同環境去覆蓋要改的地方（overlay）
```

覆蓋的地方產生mainfest.yaml, 最後在從`manifests.yaml`去作變更

`kustomize build overlays/dev > ./kustomization/manifests.yaml`

```
manifests.yaml它會是一份「完整可部署的 K8s YAML」，像這樣：

apiVersion: v1
kind: Namespace
...
---
apiVersion: apps/v1
kind: Deployment
...
---
apiVersion: v1
kind: Service
...

```
