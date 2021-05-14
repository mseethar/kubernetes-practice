# Taints and tolerations
Pods to nodes relationships.  
Restricting what Pods are placed on what node
## Taints are set on Nodes
 > $ kubectl taint nodes \<*node-name*\> _key_=_value_:*taint-effect*  
- taint-effect is what happens when the pod does not tolerate the taint
  - __NoSchedule__
    - pods will not be scheduled in the node if they do not tolerate the taint
  - __PreferNoSchedule__
    - The system will try not to schedule the pod in the node. But it is not guaranteed.
  - __NoExecute__
    - No pod that is intolerant will nto be scheduled on this node and existing pods that do not tolerate the taint will be evicted from the node

` $ kubectl taint nodes _my-high-mem-node_ _app_=_red_:_NoSchedule_`  

## Removing a taint on a node
Can be done in two ways
1. Edit the node
> kubectl edit node <_node_name_>   

2. With imperative command (note the minus at the end)
> kubectl taint node \<_node-name_> \<_taint-directive_>**-**   
> kubectl taint node master node-role.kubernetes.io/master:NoSchedule**-**   


## Tolerations are set on Pods
**Under spec of _Pods_**  
```yaml
tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```
> ~~Please note. The values should be surrounded by double quotes~~.   

Taints and tolerations indicate to a node what pods to run in them. It does not restrict a pod from being scheduled on other nodes that are untainted. This is achieved with __node affinity__.   

The scheduler, by default, does not schedule any pod on the _master_ node. Thia is because the kubernetes system by default taints the master node.   
This can be changed, but not recommended.   

On a normal Kubernetes installtion this will look like,
```shell
$ kubectl describe node kubemaster | grep -i taint
Taints:             node-role.kubernetes.io/master:NoSchedule
```   
On a minikube though, the pods are scheduled on the one and only node that acts as both _master_ and _worker_.
``` shell
$ kubectl describe node minikube | grep -i taint
Taints:             <none>
```

### Commands tail-end
`$ kubectl get nodes`   

`$ kubectl describe node node01 | grep Taint`   

`$ kubectl taint nodes node01 spray=mortein:NoSchedule`   

`$ kubectl run mosquito --image=nginx`   

`$ kubectl get pods`   

`$ kubectl describe node controlplane | grep -i taint`   

```yaml
# cat bee-pod.yaml
apiVersion: v1
metadata:
    name: bee

kind: Pod

spec:
    containers:
        - name: nginx
          image: nginx
    tolerations:
        - key: "spray"
          operator: "Equal"
          value: "mortein"
          effect: "NoSchedule"
```