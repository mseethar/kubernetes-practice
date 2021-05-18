# Jobs

Kubernetes tries to keep the pods aleays running.
The __*restartPolicy*__ of the pod is by default set to **_Always_**

> _**POD**_.spec.restartPolicy: _Always_  

Enum: _Always_, _Never_ and _OnFailure_

```yaml
apiVersion: batch/v1
kind: Job
metadata:
   name: math-add-job

spec:  # This Spec is similar to the Pod spec
   containers:
      - image: ubuntu
        name: ubuntu-batch-container
        command: ['expr', '3', '+', '2' ]
   restartPolicy: Never
```