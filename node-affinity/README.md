# Node affinity

```yaml
apiVersion: v1
kind: Pod

metadata:
   name: my-app-pod

spec:
   containers:
      - name: my-container
        image: my-image
    affinity:
       nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
             nodeSelectorTerms:
                - matchExpressions:
                    - key: size
                      operator: In
                      values:
                        - Large
                        - Medium
```   
Operator can be **_In_**, _**NotIn**_, **_Exists_** (will check if a label of this key is present)   
TODO: Check all the available operators
## Node affinity types
### Available
1. _required_ During __Scheduling__ _Ignored_ __DuringExecution__   
    The affinity requirement in the Pod definition is mandatory and the scheduler should create this pod on the nodes that match the affinity requirement.    
2. _preferred_ During __Scheduling__ _Ignored_ __DuringExecution__  
    The affinity requirement is prefereable and if the scheduler doesn't find a node that satisfy the affinity requirement of this newly created pod, it can create it on any other node that does not conform to the affinity requirements, as well.
### Planned
3. _required_ During __Scheduling__ _Required_ __DuringExecution__
    The affinity requirement is mandatory during both new creations and executions. When a pod goes out of affinity on a node while it is executing in it, it is evicted immediately.