# Jobs

Kubernetes tries to keep the pods aleays running.
The __*restartPolicy*__ of the pod is by default set to **_Always_**

> _**POD**_.spec.restartPolicy: _Always_  

Enum: _Always_, _Never_ and _OnFailure_

```yaml
apiVersion: batch/v1
kind: Job
metadata:
   name: reporting-job

spec:  # This Spec is similar to the Pod spec
    # There are 4 pods created.
    # They are created serially (if the parallelism is not set)
    # (meaning, only when one pod completes successfully the next pod is created)
    completions: 4
  # parallelism: 3   # 3 pods are run parallely
    backoffLimit: 6  # How many times to restart to get the job to a successful completion state.
    template:
        spec:
            containers:
            - image: ubuntu
                name: ubuntu-batch-container
                command: ['expr', '3', '+', '2' ]
            restartPolicy: Never
```

> $ `kubectl create job <job-name> --image <image-name>[<:label>]`

> $ `kubectl get jobs`  

> $ `kubectl get pods`  

> $ `kubectl logs <pod-name>`

> $ `kubectl delete job <job-name>`

# Cronjobs

> $ `kubectl create cronjob <cron-job-name> --image <image-name>[<:label>] --schedule '30 21 * * *'`

```yaml
apiVersion: batch/v1beta1
kind: Job
metadata:
   name: reporting-cron-job
spec:
    schedule: "*/1 * * * *"
    jobTemplate:
        spec:
            completions: 4
        # parallelism: 3   # 3 pods are run parallely
            template:
                spec:
                    containers:
                    - image: ubuntu
                        name: ubuntu-batch-container
                        command: ['expr', '3', '+', '2' ]
                    restartPolicy: Never
```

> $ `kubectl get cronjob `