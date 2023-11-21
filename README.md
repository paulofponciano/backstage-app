# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

```sh
Running on Amazon EKS !
```

Token for backstage ingestion k8s plugin:

```sh
kubectl -n kube-system create serviceaccount backstage-ingestion
kubectl create clusterrolebinding backstage-ingestion --clusterrole=cluster-admin --serviceaccount=kube-system:backstage-ingestion
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: backstage-ingestion
  namespace: kube-system
  annotations:
    kubernetes.io/service-account.name: backstage-ingestion
type: kubernetes.io/service-account-token
EOF
kubectl -n kube-system get secret backstage-ingestion -o go-template='{{.data.token | base64decode}}'
```
