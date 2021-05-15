# Pods
## Container **readiness** probe
Probes are defined at the container definition level.  
There are three kinds of _readiness_ probes.  
Kubernetes does not mark a pod as ready until the probes return a successful response.
1. Web Probe

```yaml
readinessProbe:
   httpGet:
      path: /api/ready
      port: 8080
   initialDelaySeconds: 10   # Wait for this many seconds before polling for readiness.
   periodSeconds: 5   # Interval between consecutive polling.
   failureThreshold: 10   # Number of retry attempts before marking the pod as failed.
```
2. TCP Probe

```yaml
readinessProbe:
   tcpSocket:
      port: 3306
```
3. Command / script probe  

```yaml
readinessProbe:
   exec:
      command:
         - cat
         - /usr/local/bin/is_app_ready
```

## Container **liveness** probe
Probes are defined at the container definition level.  
There are three kinds of _liveness_ probes.  
If the liveness probe returns failure status, the Kubernetes marks the pod as failed and evicts the failed pod and recreates a new one.

__*successThreshold*__ can also be configured for liveness in pods.

1. Web Probe

```yaml
livenessProbe:
   httpGet:
      path: /api/ready
      port: 8080
   initialDelaySeconds: 10   # Wait for this many seconds before polling for liveness.
   periodSeconds: 5   # Interval between consecutive polling.
   failureThreshold: 10   # Number of retry attempts before marking the pod as failed.
```
2. TCP Probe

```yaml
livenessProbe:
   tcpSocket:
      port: 3306
```
3. Command / script probe  

```yaml
livenessProbe:
   exec:
      command:
         - cat
         - /usr/local/bin/is_app_ready
```