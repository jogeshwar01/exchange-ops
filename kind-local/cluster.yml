kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30007     # need this as kind runs on docker 
    hostPort: 30007
  - containerPort: 30008
    hostPort: 30008
- role: worker
- role: worker