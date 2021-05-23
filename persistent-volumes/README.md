# Persistent Volumes
TODO: Reinforce  
Can be shared across Pods.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
   name: pv-vol1

spec:
    accessModes:
      - ReadWriteOnce      # ReadOnlyMany, ReadWriteMany
    capacity:
        storage: 1Gi
    awsElasticBlockStore:
        volumeID: <volume-id>
        fsType: ext4
```

# Persistenc Volume Claims

persistentVolumeReclaimPolicy
 - Retain
 - Delete
 - Recycle

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 8Gi
  storageClassName: slow
  selector:
    matchLabels:
      release: "stable"
    matchExpressions:
      - {key: environment, operator: In, values: [dev]}
```
<details>
<summary>Add a persistent volume claim to a Pod - YAML</summary>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

</details>

TODO: How do the PV and PVC are matched?
TODO: How do the accessModes affect the PV and PVC matching?

# Static provisioning

Creating a _Persistent Volume_ and a _Persistent Volume Claim_, and associating the Persistent Volume Claim to a Pod is static provisioning.

The volume is expected to exist when a claim is created (and associated with a Pod).

# Dynamic provisioning

Dynamic provisioning is done by defining a StorageClass

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
    name: google-storage

provisioner: kubernetes.io/gce-pd
```

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: google-storage-claim

spec:
    accessModes:
      - ReadWriteOnce
    storageClassName: google-storage
    resources:
        requests:
            storage: 500Mi
```

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: google-storage-claiming-pod

spec:
    containers:
        - image: alpine
          name: alpine-container
          command: [ "/bin/sh", "-c" ]
          args: [ "shuf -i 0-100 -n 1 > /opt/prediction.txt" ]
          volumeMounts:
            - mountPath: /opt
              name: data-volume
    volumes:
    - name: data-volume
      persistentVolumeClaim:
          claimName: google-storage-claim
```

> $ `kubectl get storageclasses.storage.k8s.io`