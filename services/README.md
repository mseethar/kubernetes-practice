# Services
Services enable connectivity between pods.  

> $ `kubectl create svc nodeport webapp-service --tcp=<port>:<targetPort> --node-port <nodePort> --dry-run=client -o yaml`

> $ `kubectl create svc nodeport webapp-service --tcp=8080:8080 --node-port 30080 --dry-run=client -o yaml`

> $ `kubectl expose deployment <deployment-name> --name=<service-name> --target-port=8080 --port=8080 --type=NodePort  --dry-run=client -o yaml`

> $ `$ kubectl expose (-f FILENAME | TYPE NAME) [--port=port] [--protocol=TCP|UDP|SCTP] [--target-port=number-or-name] [--name=name] [--external-ip=external-ip-of-service] [--type=type]`