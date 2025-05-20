#### Git Repo

https://github.com/bitnami-labs/sealed-secrets
https://www.youtube.com/watch?v=wWMJCY2E0d4&pp=ygUYc2VhbGVkIHNlY3JldHMga29kZWtsb3Vk

#### Installation

https://artifacthub.io/packages/helm/bitnami-labs/sealed-secrets

1. install helm chart

```sh
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets
```

2. install kubeseal

```sh
KUBESEAL_VERSION='0.29.0' // check latest version
curl -OL "https://github.com/bitnami-labs/sealed-secrets/releases/download/v${KUBESEAL_VERSION:?}/kubeseal-${KUBESEAL_VERSION:?}-linux-amd64.tar.gz"
tar -xvzf kubeseal-${KUBESEAL_VERSION:?}-linux-amd64.tar.gz kubeseal
sudo install -m 755 kubeseal /usr/local/bin/kubeseal
```

Check installation -

```sh
kubeseal --fetch-cert
```

3. Create Secret yml

4. Create Sealed secret

```sh
kubeseal --format yaml < sample-secret.yml > sealed_secret.yml
```

5. Check the sealed secret

```sh
kubectl apply -f sealed_secret.yml
kubectl get secret
kubectl get secret exchange-router-secret -o yaml
```

#### Instructions

You should now be able to create sealed secrets.

1. Install the client-side tool (kubeseal) as explained in the docs below:

   https://github.com/bitnami-labs/sealed-secrets#installation-from-source

2. Create a sealed secret file running the command below:

   kubectl create secret generic secret-name --dry-run=client --from-literal=foo=bar -o [json|yaml] | \
   kubeseal \
    --controller-name=sealed-secrets-controller \
    --controller-namespace=kube-system \
    --format yaml > mysealedsecret.[json|yaml]

The file mysealedsecret.[json|yaml] is a commitable file.

If you would rather not need access to the cluster to generate the sealed secret you can run:

    kubeseal \
      --controller-name=sealed-secrets-controller \
      --controller-namespace=kube-system \
      --fetch-cert > mycert.pem

to retrieve the public cert used for encryption and store it locally. You can then run 'kubeseal --cert mycert.pem' instead to use the local cert e.g.

    kubectl create secret generic secret-name --dry-run=client --from-literal=foo=bar -o [json|yaml] | \
    kubeseal \
      --controller-name=sealed-secrets-controller \
      --controller-namespace=kube-system \
      --format [json|yaml] --cert mycert.pem > mysealedsecret.[json|yaml]

3. Apply the sealed secret

   kubectl create -f mysealedsecret.[json|yaml]

Running 'kubectl get secret secret-name -o [json|yaml]' will show the decrypted secret that was generated from the sealed secret.

Both the SealedSecret and generated Secret must have the same name and namespace.
