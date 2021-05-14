# Node selectors and labels   

## Labeling a node
> $ kubectl label nodes \<_node-name_> <_label-key_>=<_label-value_>   

> $ kubectl label nodes _**node-01**_ _**size**_=_**Large**_   

## Pod config
```yaml
apiVersion: v1
kind: Pod

metadata:
    name: data-processor-pod
    label: data-processor

spec:
    containers:
        - name: kafka
          image: kafka:latest
    nodeSelector:
        size: Large # Selects the node that has a lable 'size' with value 'Large'
```

## Caveat
Node selectors can't be used to express complex selection requirement. For example label value being 'Medium' or 'Large'.

For that _Node affinity_ and _anti-affinity_ configurations come in as handy.