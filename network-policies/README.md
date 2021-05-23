# Network policies

> $ `kubectl get netpol`

> $ `kubectl get networkpolicy`

> $ `kubectl get networkpolicies`

> $ `kubectl get networkpolicies.networking.k8s.io`

> Network policies are enforced by Network solutions.  
The following solutions support.
1. Kube-router
1. Calico
1. Romana
1. Weave-net
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy

metadata:
    name: db-policy

spec:
    podSelector:    # The Pods this ingress rule to be applied to
        matchLabels:
            role: db
    policyTypes:
      - Ingress
    ingress:
      - from:      # This is an OR among the children of this section
          - podSelector:     # This is an AND among the children of this section.
              matchLabels:
                  name: api-pod
            namespaceSelector:
              matchLabels:
                  name: prod
          - ipBlock:
              cidr: 192.168.5.10/32
        ports:
          - protocol: TCP
            port: 3306
```