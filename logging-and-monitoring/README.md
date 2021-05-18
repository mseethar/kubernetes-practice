# Logging

## Logging in containers
For single container pods   
 > ```$ kubectl logs -f <_pod-name_>```

For __multi__-container pods  

Lists the containers in the pod  
 > ```$ kubectl logs -f <_pod-name_> -c```  

Getting the logs from a specific container  
 > ```$ kubectl logs -f <_pod-name_> <_container-name_>```

 > ```$ kubectl logs -f <_pod-name_> -c <_container-name_>```  
 

 # Monitoring

 1. Elastic stack
 2. Heapster - Deprecated
 3. Metrics server
    - Scaled down version of Heapster
    - Gathers metrics from nodes and pods
    - Store in memory (no disk storage and no history / trend data)
    - Kubelet with it's cAdviser component gathers the metrics and  
       makes available to the metrics server.  
       - To enable metrics server on minikube run the following command.  
         > ```minikube addons enable metrics-server```  

       - For other environments clone metrics-server from github and deploy it  
       https://github.com/kubernetes-sigs/metrics-server  

         - _Option 1_
           > ```git clone https://github.com/kubernetes-incubator/metrics-server.git```  
           > ```kubectl create -f deploy/1.8+/```  

         - _Option 2_
           > ```git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git```  

    - After installing  
    > ```$ kubectl top nodes```  
    > ```$ kubectl top pods```  
 4. Prometheus
 5. Datadog
 6. Dynatrace
