# Ingress

## Ingress controller
This is the Layer 7 load balancer or reverse proxy like _GCE (Google Cloud Layer 7 load balancer)_, _nginx_, _haproxy_, _contour_, _istio_ or _traefic_.

Support for nginx, AWS and GCE Ingress Controllers are maintained by Kubernetes.

Nginx ingress controller options - https://kubernetes.github.io/ingress-nginx/examples/

Ingress controller needs appropriate service account creates with required roles assigned.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-ingress-controller

spec:
    replicas: 2
    selector:
        matchLabels:
            name: nginx-ingress
    template:
        metadata:
            labels:
                name: nginx-ingress
        spec:
            containers:
                - name: nginx-ingress-controller
                  image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
            args:
                - /nginx-ingress-controller
                - --configmap=$(POD_NAMESPACE)/nginx-configuration   # nginx-configuration is the name of the kubernetes ConfigMap that has the nginx configuration.
            env:
                - name: POD_NAME
                  valueFrom:
                      fieldRef:
                          fieldPath: metadata.name
                - name: POD_NAMESPACE
                  valueFrom:
                      fieldRef:
                          fieldPath: metadata.namespace
            ports:
                - name: http
                  containerPort: 80
                - name: https
                  containerPort: 443
---
# Service to expose the nginx to the external world
---
apiVersion: v1
kind: Service
metadata:
    name: nginx-ingress

spec:
    type: NodePort
    ports:
      - port: 80
        targetPort: 80
        protocol: TCP
        name: http
      - port: 443
        targetPort: 443
        protocol: TCP
        name: https
    selector:
        name: nginx-ingress

---
# Service account
# Roles, ClusterRoles, RoleBindings
---
apiVersion: v1
kind: ServiceAccount
metadata:
    name: nginx-ingress-service-account
```

## Ingress resources
These are the routing rules created as kubernetes resource definitions.

> $ `kubectl get ingress -o wide -n app-space`

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-wear

spec:
    backend:
        serviceName: wear-service
        servicePort: 80
```

> $ `kubectl get ingress`

> $ `kubectl get ingress --all-namespaces`

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-wear

spec:
    rules:
      - http:
          paths:
            - path: /wear
              backend:
                  serviceName: wear-service
                  servicePort: 80
            - path: /watch
              backend:
                  serviceName: watch-service
                  servicePort: 80
```

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-wear

spec:
    rules:
      - host: wear.my-online-store.com
        http:
          paths:
            - backend:
                  serviceName: wear-service
                  servicePort: 80
      - host: watch.my-online-store.com
        http:
          paths:
            # - path: /watch - Matches all the path
            - backend:
                  serviceName: watch-service
                  servicePort: 80
```

Advanced rewrite targets (specific for nginx ingress controller)

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
  name: rewrite
  namespace: default
spec:
  rules:
  - host: rewrite.bar.com
    http:
      paths:
      - backend:
          serviceName: http-svc
          servicePort: 80
        path: /something(/|$)(.*)
```