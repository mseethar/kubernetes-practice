# Design patterns
1. Sidecar  
   Ex. logger agent
2. Adapter  
   Ex. Log format normalizer
3. Ambassador  
   Ex. DB router based on environment (like dev, qa and prod)


```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: yellow
  name: yellow
spec:
  containers:
  - image: busybox
    name: lemon
  - image: redis
    name: gold
```