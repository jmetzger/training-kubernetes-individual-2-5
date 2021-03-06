# Simple Example Calico/Cilium 

```
# Schritt 1:
kubectl create ns policy-demo<tln>
kubectl create deployment --namespace=policy-demo<tln> nginx --image=nginx
kubectl expose --namespace=policy-demo<tln> deployment nginx --port=80
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo<tln> access --rm -ti --image busybox -- sh
```
```
# innerhalb der shell 
wget -q nginx -O -
```


```
# Schritt 2: Policy festlegen, dass kein Ingress-Traffic erlaubt
# in diesem namespace: policy-demo 
# cd; mkdir -p manifests/network; cd manifests/network 
# vi 01-policy.yml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: a-simple-policy
  namespace: policy-demo-<tln>
spec:
  podSelector:
    matchLabels: {}
```


```
kubectl apply -f 01-policy.yml 
```

```
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
```

```
# innerhalb der shell 
# kein Zugriff möglich
wget -q nginx -O -
```

```
# Schritt 3: Zugriff erlauben von pods mit dem Label run=access 
# 02-allow.yml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-nginx
  namespace: policy-demo<tln>
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
    - from:
      - podSelector:
          matchLabels:
            run: access
```


```
kubectl apply -f 02-allow.yml 
```

```
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
# pod hat durch run -> access automatisch das label run:access zugewiesen 
kubectl run --namespace=policy-demo-<tln> access --rm -ti --image busybox /bin/sh
```

```
# innerhalb der shell 
wget -q nginx -O -
```

``` 
kubectl run --namespace=policy-demo no-access --rm -ti --image busybox /bin/sh
```

```
# in der shell  
wget -q nginx -O -
```

```

kubectl delete ns policy-demo-<tln> 

```


## Ref:

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-basic
