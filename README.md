# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

```sh
Running on Amazon EKS !
```

Backstage extra plugins used:

  https://backstage.io/docs/features/kubernetes/installation,
  https://backstage.io/docs/reference/integration-aws-node/,
  https://backstage.io/docs/getting-started/homepage/

```sh
yarn add --cwd packages/backend @backstage/plugin-kubernetes-backend
yarn add --cwd packages/app @backstage/plugin-kubernetes
yarn add --cwd packages/app @backstage/integration-aws-node
yarn add --cwd packages/app @backstage/plugin-home
```

Secrets needed for k8s (https://github.com/paulofponciano/backstage-app/tree/main/k8s):

```sh
github-auth-secrets
  AUTH_GITHUB_CLIENT_ID
  AUTH_GITHUB_CLIENT_SECRET

eks-secrets
  SA_TOKEN
  CA_DATA

backstage-secrets
  GITHUB_TOKEN

postgres-secrets
  POSTGRES_USER
  POSTGRES_PASSWORD

backstage-ingestion
  token
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

