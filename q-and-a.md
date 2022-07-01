# Questions and Answers 

## Wieviele Replicaset beim Deployment zurückbehalt / Löschen von Replicaset 

```

kubectl explain deployment.spec.revisionHistoryLimit 

apiVersion: apps/v1
kind: Deployment
# ...
spec:
  # ...
  revisionHistoryLimit: 0 # Default to 10 if not specified
  # ...
  
```

## Wo dokumentieren, z.B. aus welchem Repo / git 

```
Labels can be used to select objects and to find collections of objects that satisfy certain conditions. In contrast, annotations are not used to identify and select objects. 
```
  * https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/
  * https://kubernetes.io/docs/reference/labels-annotations-taints/

## Wie kann ich die Default Class für Ingress bzw. IngressClass setzten 

```
apiVersion: networking.k8s.io/v1
kind: IngressClass
metadata:
  labels:
    app.kubernetes.io/component: controller
  name: nginx-example
  annotations:
    ingressclass.kubernetes.io/is-default-class: "true"
spec:
  controller: k8s.io/ingress-nginx
```

## Wie kann ich bei docker einloggen mit kubernetes bzw. ...

```
# einen pod von einem image aus einer registry mit credentials ziehen.
# klassische / unsichere Lösung / weil credentials in manifest gespeichert werden (unverschlüsselt) 
https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
```

## Sind Änderungen in einer eingehängt config-map sofort im pod sichtbar ? 

```
Nein 
```

```
Lösung als Workaround: https://github.com/stakater/Reloader
```
