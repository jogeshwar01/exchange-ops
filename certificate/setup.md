1. install helm

https://helm.sh/docs/intro/install/

2. install cert-manager

https://cert-manager.io/docs/installation/helm/

3. Commands

kubectl get clusterissuer
kubectl describe clusterissuer letsencrypt-prod

4. Apply yml

kubectl apply -f issuer.yml
kubectl apply -f certificate.yml

5. Check

kubectl get certificate
kubectl describe certificate exchange-cert
