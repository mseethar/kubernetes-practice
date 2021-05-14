# kubernetes-practice
## Resources
1. pods
2. services  
   1. NodePort
   2. ClusterIP
   3. LoadBalancer
3. deployments
4. 

### Listing resources
```shell
$ kubectl get all
$ kubectl get pods
$ kubectl get services
```
### Describe a resource
```shell
$ kubectl describe nodes node01
$ kubectl describe pod my-new-pod
```

## Nodes
Labeling a node  
```shell
$ kubectl label nodes node01 color=blue
```

## Deployments

> __kubectl create deployment blue --image=nginx --replicas=3 --dry-run=client -o yaml__  

```yaml
# root@controlplane:~# kubectl create deployment blue --image=nginx \
#               --replicas=3 --dry-run=client -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: blue
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: blue
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: blue
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

## TODO: Check on the error message
```Warning: resource deployments/blue is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.```