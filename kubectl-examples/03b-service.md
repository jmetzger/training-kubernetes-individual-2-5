# Example Service 

```
# cd; mkdir -p manifests/service; cd manifests/service 
# nano 01-nginx.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 2
  template:
    metadata:
      labels:
        run: my-nginx
    spec:
      containers:
      - name: cont-nginx
        image: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: svc-nginx
  labels:
    run: svc-my-nginx
spec:
  type: ClusterIP
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx               
```        

```
kubectl apply -f . 
kubectl get deploy; kubectl get po -o wide; kubectl get service -o wide 
```


## Ref.

  * https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/
