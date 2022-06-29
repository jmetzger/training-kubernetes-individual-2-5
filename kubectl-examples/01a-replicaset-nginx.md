# Replicaset

```
# cd; mkdir -p manifests/rs; cd manifests/rs
# nano 01-rs.yml 
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replica-set
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      name: nginx-pods
      labels:
        tier: frontend
    spec:
      containers:
        - name: nginx
          image: "nginx:latest"
          ports:
             - containerPort: 80
            
```

```
kubectl apply -f .
kubectl get rs -o wide
kubectl get po -o wide 
```
