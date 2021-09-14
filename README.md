# argo-cd

This repository demonstrates a simple use of Argo CD (a declarative GitOps CD for kubernetes) with an example application deployed in the same cluster (along with Argo CD installation).

## Argo CD installation

Connect to a kubernetes cluster and install Argo CD:
```
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify argo is up and running:
```
kubectl -n argocd get pods
```

Port forward to allow access to Argo API server through Argo UI (http://localhost:8080):

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Initial auto-genetaed password for admin account is saved as a secret in `argocd` namespace and can be extracted using:

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```

## Create and manage applications

Create a namespace `app-bookinfo` where an example app will be deploed:
```
kubectl apply -f app-bookinfo/namespace.yaml
```

Create an Argo application:
```
kubectl apply -f app.yaml
```

You will now see an application `bookinfo` created and respective kubernetes resource being created or updated at http://localhost:8080

## GitOps continuous deployment in action

Add an additional deployment the above application by adding a new `reviews-deploy-v3.yaml` file under `app-booinfo/reviews` folder. Push your local changes to remote.

You will now see additional deployment and pods created in argo CD UI.
