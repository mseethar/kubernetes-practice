# Stateful sets
- When a predictable names for Pods are required
- When Pod creation should be serialized

- Does an ordered, graceful deployment
- Assigns stable and unique network identifiers

> Definition is the same as a **_Deployment_**, with an extra service name of a headless service under the **_spec_**.

> Scaling up and down of a Stateful set behaves the same way as that of creation as well.

1. Scaling up creates replicas sequentially with the ordered Pods creation and naming

1. Scaling down deletes the Pods from latest created Pod. That is in the reverse order they were created.

1. Deleting a Stateful set deletes the Pods in the reverse order they were created.

> $ `kubectl scale statefulset mysql replicas=5`  

> $ `kubectl scale statefulset mysql replicas=3`

> $ `kubectl delete statefulset mysql`

> Ordered launch can be overridden to un-ordered and parallel launches, retaining the stable and unique identifiers assignments of a _Stateful set_. Set the __*podManagementPolicy*__ field to __*Parallel*__ to achieve this.

TODO: Learn more on the podManagementPolicy more.

# Headless services

1. Used when we do not want the load-balancing functionality provided by the normal service.
1. Creates a service for each pod with \<pod-name>.\<headless-service-name>.\<namespace>.svc.cluster.local

<details>
<summary>Define headless service - YAML</summary>

```yaml
apiVersion: v1
kind: Service
metadata:
    name: mysql-h

spec:
    ports:
    - port: 3306
    selector:
        app: mysql
    clusterIP: None      # The value None for clusterIP defines the service as a headless service.
```
</details>

<details>
<summary>Use the headless service for a Pod - YAML</summary>

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: mysql-pod
    labels:
        app: mysql

spec:
    containers:
    - image: mysql
      name: mysql-container
    
    subdomain: mysql-h    # This name should be the name of the headless service.
    hostname: mysql-pod   # This is mandatory for the DNS A record creation, for THIS POD.
```
</details>

The above Pod definition cannot be used in a _deployment_. Because, the deployment will assign the same DNS. That is not a desirable outcome.

This is where StatefulSet come in. For a StatefulSet, instead of configuring the subdomain and the hostname, the service name is configured directly as shown below.

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
    name: mysql-stateful-set
    labels:
        app: mysql

spec:
    serviceName: mysql-h      # StatefulSets need the serviceName explicitly
    replicas: 3
    matchLabels:
        app: mysql
    template:
        metadata:
            name: mysql-pod
            labels:
                app: mysql
        spec:
            containers:
            - image: mysql
              name: mysql-container
    volumeClaimTemplates:     # This creates dedicated volumes for each of these pods
    - metadata:
        name: data-volume
      spec:
          accessModes:
          - ReadWriteOnce
          storageClassName: google-storage
          resources:
              requests:
                  storage: 500Mi
```
