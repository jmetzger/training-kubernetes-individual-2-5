# Busybox zum Debuggen von anderen Pods 

```
# Rahmenbedingungen.
# Gemanaged'er Kubectl - Cluster und ich kann nicht per ssh zugreifen 
# Lösung - busybox starten und damit Verbindungen testen.
kubectl run --rm -it busybux --image busybox -- sh 

# Jetzt kann ich mich so verhalten als wäre ich im Cluster 
# z.B. 
# ping <andere-pod-ip>
```
