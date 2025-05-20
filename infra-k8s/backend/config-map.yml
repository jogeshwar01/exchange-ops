apiVersion: v1
kind: ConfigMap
metadata:
  name: exchange-router-config
data:
  server_addr: "0.0.0.0:8080"
  database_url: "postgres://root:root@exchange-postgres-service.default.svc.cluster.local:80/exchange-db"
  redis_url: "redis://exchange-redis-service.default.svc.cluster.local:80"
