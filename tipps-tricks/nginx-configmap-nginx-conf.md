# Use configmap for configuration of nginx 

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-conf
data:
  nginx.conf: |
    # jochens personal config map 
    user nginx;
    worker_processes  1;
    events {
      worker_connections  10240;
    }
    http {
      server {
        listen       80;
        server_name  localhost;
        location / {
          root /usr/share/nginx/html;
        }
      }
    }
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
        volumeMounts:
            - name: nginx-conf-vol
              mountPath: /etc/nginx/nginx.conf
              subPath: nginx.conf
              readOnly: true
      volumes:
      - name: nginx-conf-vol
        configMap:
          name: nginx-conf
          items:
            - key: nginx.conf
              path: nginx.conf



```
