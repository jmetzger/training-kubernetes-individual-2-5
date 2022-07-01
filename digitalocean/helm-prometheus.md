# Installation prometheues with helm (DigitalOcean Kubernetes) 

## Set the custom-values 

```
# There are some restrictions on the digitalocean kubernetes cluster
# and we need to take these into account 
```

```
# cd; mkdir manifests/helm-prometheus; cd manifests/helm-prometheus
# vi values.yml
# Define persistent storage for Prometheus (PVC)
prometheus:
  prometheusSpec:
    storageSpec:
      volumeClaimTemplate:
        spec:
          accessModes: ["ReadWriteOnce"]
          storageClassName: do-block-storage
          resources:
            requests:
              storage: 5Gi

# Define persistent storage for Grafana (PVC)
grafana:
  # Set password for Grafana admin user
  adminPassword: your_admin_password
  persistence:
    enabled: true
    storageClassName: do-block-storage
    accessModes: ["ReadWriteOnce"]
    size: 5Gi

# Define persistent storage for Alertmanager (PVC)
alertmanager:
  alertmanagerSpec:
    storage:
      volumeClaimTemplate:
        spec:
          accessModes: ["ReadWriteOnce"]
          storageClassName: do-block-storage
          resources:
            requests:
              storage: 5Gi

# Change default node-exporter port
prometheus-node-exporter:
  service:
    port: 30206
    targetPort: 30206

# Disable Etcd metrics
kubeEtcd:
  enabled: false

# Disable Controller metrics
kubeControllerManager:
  enabled: false

# Disable Scheduler metrics
kubeScheduler:
  enabled: false

```

```
# setup helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

```
# install helm chart 
# Unfortunately this does not work with digitalocean managed kubernetes (doks) - 1.22.
# helm install --namespace monitoring --create-namespace -f values.yml prometheus  prometheus-community/kube-prometheus-stack
# Error with EOF 

# But you can do this 
helm template --namespace monitoring --create-namespace -f values.yml prometheus  prometheus-community/kube-prometheus-stack > all.yml
kubectl apply -f all.yml 


# now check, if everything is up and running // takes a couple of minutes  
kubectl -n monitoring get all 

```

```
# Access Grafana 

# Grafana: (Version kubectl on your client) 

# in our case kubectl is on remote client, so we need open a tunnel 
# Session 1:
kubectl port-forward -n monitoring svc/prometheus-grafana 8000:80

# Session 2:
# Service Listens on 8000
ssh -L 8000:localhost:8000 tln<tln>@<ip-client>

# In browser:
http://localhost:8000
user: admin
pass: <from-values-files-above>

```

```
# Alternative Quick hack, change service to nodeport 
kubectl -n monitoring get svc/prometheus-grafana -o yaml > 01-svc-grafana.yml
# change type -> to -> NodePort 
kubectl apply -f 01-svc-grafana.yml 
kubectl -n monitoring get svc/prometheus-grafana

# IP of one Node 
http://68.183.216.6:30220/

```







## Reference

  * A bit outdated, be cause the name of the chart has changed
    * https://www.digitalocean.com/community/tutorials/how-to-set-up-digitalocean-kubernetes-cluster-monitoring-with-helm-and-prometheus-operator
