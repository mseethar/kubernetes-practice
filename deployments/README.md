# Deployments
## New deployment / Create

> $ `kubectl create -f <deployment-definition>.yaml`  

> $ `kubectl create -f <deployment-definition>.yaml --record`  

## Updates and rollbacks

> $ `kubectl apply -f <deployment-definition>.yaml`  



## Rollouts

> Each deployment creates a new replicaset.

> $ `kubectl rollout status deployment/<deplyment-name>`  

> $ `kubectl rollout history deployment/<deployment-name>`  

> $ `kubectl rollout history deployment nginx --revision=1`  

> $ `kubectl set image deployment/<deployment-name> <container-name>=<container-image>:<version> --record`  

> $ `kubectl set image deployment/<deployment-name> nginx-container=nginx:1.9.1 --record`

### Strategies
#### _Recreate_
 - Outgoing instances are removed
 - New instances are deployed
 - There is an application down time involved

#### _Rolling update_
 - Each instance is removed and in its place
 - A new instance is created.
   - This is repeated until all the instances are replaced

> $ `kubectl get replicasets`

## Rollbacks

> $ `kubectl rollout undo deployment/<deployment-name>`