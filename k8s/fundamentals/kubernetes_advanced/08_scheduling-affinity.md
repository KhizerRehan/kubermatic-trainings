# Node Affinities
In this course will show how the K8s Scheduler tries to keep things away from each other

1. Create a Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fire
  labels:
    element: fire
spec:
  containers:
    - name: fire
      image: nginx
```
```bash
kubectl create -f fire.yaml
```
2. Create a Deployment which wants to keep distance to the first Pod.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: water
spec:
  replicas: 10
  selector:
    matchLabels:
      app: water
  template:
    metadata:
      labels:
        app: water
    spec:
      containers:
        - name: water
          image: nginx
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                  - key: element
                    operator: In
                    values:
                      - fire
              topologyKey: "kubernetes.io/hostname"  
```
```bash
kubectl create -f water.yaml
```
3. Verify that the Pods `water` are not on the same node like the node `fire`
```bash
kubectl get pods -o wide
```
