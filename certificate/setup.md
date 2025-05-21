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

kubectl get secret exchange-tls -n default -o yaml
kubectl get secret exchange-tls -n default -o jsonpath='{.data.tls\.crt}' | base64 --decode | openssl x509 -text -noout

6. Youtube

See steps - do not copy the same cert-manager ymls though

https://www.youtube.com/watch?v=DvXkD0f-lhY&pp=ygUXY2VydCBtYW5hZ2VyIGt1YmVybmV0ZXM%3D

7. Here we don't need the certificate as we are defining it inside the ingress yml

- if you already had created a certificate - first remove the file and then delete corresponding secret

kubectl delete certificate exchange-tls
kubectl delete secret exchange-tls
