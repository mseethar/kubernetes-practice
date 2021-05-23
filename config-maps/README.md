# ConfigMaps

```shell
$ kubectl create configmap <config-map-name> --from-literal=<KEY>=<KEY_VALUE> \  
                                --from-literal=<KEY1>=<KEY_VALUE1>
```

```shell
$ kubectl create configmap <config-map-name> --from-literal=<properties-file-path>
```

```shell
$ kubectl create configmap <config-map-name> --from-literal=<properties-file-1-path>,<properties-file-2-path>
```

```shell
$ kubectl create configmap <config-map-name> --from-literal=routing.properties,validation.properties
```

> $ `kubectl get configmaps`  

> $ `kubectl get cm`  

> $ `kubectl create configmap app-config --from-file=app.properties,app2.properties --dry-run=client -o yaml`  

> $ `kubectl -n ingress-space create cm nginx-configuration`