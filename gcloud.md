### GKE gcloud commands

1. install gcloud cli

https://cloud.google.com/sdk/docs/install-sdk

2. install kubectl

https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl

3. login gcloud

```sh
gcloud auth login
```

4. initialize gcloud

```sh
gcloud init
```

5. to switch projects

```sh
gcloud config set project [YOUR_PROJECT_ID]
```

6. get creds to use kubectl

```sh
gcloud container clusters get-credentials [CLUSTER_NAME] --location [LOCATION]
```

7. get kubectl context

```sh
kubectl config get-contexts
```

8. use a different context

```sh
kubectl config use-context kind-local
```

9. gcloud clusters list

```sh
gcloud container clusters list
```
