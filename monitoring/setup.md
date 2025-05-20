# Monitoring Setup Guide

### Initial Setup

https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

```sh
kubectl create namespace monitoring
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring
```

### Verify Installation

To check the status of kube-prometheus-stack installation:

```sh
kubectl --namespace monitoring get pods -l "release=prometheus"
```

### Access Grafana

#### Get Admin Password

To retrieve the Grafana 'admin' user password:

```sh
kubectl --namespace monitoring get secrets prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo
```

#### Access Grafana UI

To access the Grafana local instance:

```sh
export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=prometheus" -oname)
kubectl --namespace monitoring port-forward $POD_NAME 3000
```

### Access Prometheus

To access the Prometheus UI:

```sh
kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090:9090 -n monitoring
```
