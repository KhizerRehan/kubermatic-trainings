# Persistence with StorageClass

1. Create a file `persistence.yaml` with a storage class and two claims.

```yaml 
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: slow
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-standard
  zones: europe-west3-a
reclaimPolicy: Delete
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-30-slow
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: slow
  resources:
    requests:
      storage: 30Gi
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-10-slow
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: slow
  resources:
    requests:
      storage: 10Gi
```

```bash
$ kubectl create -f persistence.yaml
```

2. Run `kubectl get pv,pvc`. Take a look at the number and the names of the volumes:

```bash
$ kubectl get pv,pvc
NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                STORAGECLASS   REASON   AGE
persistentvolume/pvc-719e1838-16f3-11ea-9419-42010a8e0162   30Gi       RWO            Delete           Bound    default/pvc-30-slow   slow                    22m
persistentvolume/pvc-9f8b0df2-16f3-11ea-9419-42010a8e0162   10Gi       RWO            Delete           Bound    default/pvc-10-slow   slow                    20m
NAME                                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/pvc-10-slow   Bound    pvc-9f8b0df2-16f3-11ea-9419-42010a8e0162   10Gi       RWO            slow           20m
persistentvolumeclaim/pvc-30-slow   Bound    pvc-719e1838-16f3-11ea-9419-42010a8e0162   30Gi       RWO            slow           22m
```

3. Show the available storage classes. Possibly there are standard ones.

```bash
$ show available storage classes
kubectl get sc -o wide
```

4. Delete the PVCs
```bash
kubectl delete pvc --all
```

5. Note that besides the PVCs also the PVs got deleted due to the `reclaimPolicy` of the StorageClass
```bash
kubectl get pvc,pv
```


