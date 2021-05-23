# Volumes and mounts

```yaml
apiVersion: v1
kind: Pod

metadata:
    name: volume-pod

spec:
   volumes:
       - name: data-volume
         hostPath:
             path: /data        # Path on the host
             type: Directory
   
   containers:
     - image: alpine
       name: alpine
       command: [ "bin/sh", "-c" ]
       args: [ "shuf -i 0-100 -n 1 >> /opt/number.out;" ]
       volumeMounts:
         - mountPath: /opt
           name: data-volume
```

Other Volume types.
1. NFS
1. GlusterFS
1. Flocker
1. Ceph
1. SCALEIO
1. Google Persistent disk
1. Azure disk
1. AWS Elastic Block store volume

```yaml
volumes:
    - name: data-volume
      awsElasticBlockStore:
         volumeID: <volume-id>
         fsType: ext4
```


> What volume type is used to store data in a Pod only as long as that Pod is running on that node? When the Pod is deleted the files are to be deleted as well.