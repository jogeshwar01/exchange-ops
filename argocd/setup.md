1. Install ArgoCD

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

2. Access the argocd ui

```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

3. Get the argocd password

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
```

​ 4. Debugging

```sh
kubectl logs -n argocd deployment/argocd-application-controller
```

5. Create a new app via the UI (or via cli)
- add your repo url
- path - .
- directory - recurse
- sync policy - automated
- cluster - in-cluster
- namespace - default
- project - default