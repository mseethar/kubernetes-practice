# kubernetes-practice
## Resources
1. pods
2. services  
   1. NodePort
   2. ClusterIP
   3. LoadBalancer
3. deployments
4. labels
5. taints
6. tolerations

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

## Imperatives

### Pods
> $ ```kubectl run mosquito --image=nginx --dry-run=client -o yaml```  

#### Executing commands on a Pod's container
> $ ```kubectl --namespace elastic-stack exec app -- cat /log/app.log```  
> $ ```kubectl -n elastic-stack exec app -- cat /log/app.log```  
> $ ```kubectl -n namespace logs app```
### Services

### Nodes
> $ ```kubectl get node node01 --show-labels```

### Deployments
> $ ```kubectl get deploy```  
> $ ```kubectl get deployments```  
> $ ```kubectl get deployments.apps```  
> $ ```kubectl create deployment blue --image=nginx --replicas=3```  

> $ ```kubectl scale deployment _deployment-name_ --replicas=5```
### Labels
> kubectl label nodes node01 _label-key_=_label-value_  
### Taints
> kubectl taint node \<_node-name_> \<_taint-directive_>**-**   
> kubectl taint node master node-role.kubernetes.io/master:NoSchedule**-**   
#### Untaint
> $ kubectl taint node node01 spray=mortien:NoSchedule-
> $ kubectl taint node controlplane node-role.kubernetes.io/master:NoSchedule-  

### Tolerations
There is no imperative to configure tolerations
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: bee
  name: bee
spec:
  containers:
  - image: nginx
    name: bee
  tolerations:
    - key: spray
      operator: Equal
      value: mortein
      effect: NoSchedule
```

### Labeling
> $ ```kubectl label nodes node01 color=blue```

# Certification tips and tricks
https://www.linkedin.com/pulse/my-ckad-exam-experience-atharva-chauthaiwale/

https://medium.com/@harioverhere/ckad-certified-kubernetes-application-developer-my-journey-3afb0901014  

https://github.com/lucassha/CKAD-resources