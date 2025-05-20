apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: exchange-cert
  namespace: default
spec:
  secretName: exchange-tls
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
  commonName: exchange.jogeshwar.xyz
  dnsNames:
    - exchange.jogeshwar.xyz
