= 
POD and Deployment 
=
1) Create an NGINX Pod

    kubectl run --generator=run-pod/v1 nginx --image=nginx


2) Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

   kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml


3) Create a deployment

    kubectl create deployment --image=nginx nginx


4) Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

    kubectl create deployment --image=nginx nginx --dry-run -o yaml


5) Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)

    kubectl create deployment --image=nginx nginx --dry-run -o yaml > nginx-deployment.yaml

    Save it to a file, make necessary changes to the file (for example, adding more replicas) and then create the deployment.

= 
IMPORTANT 
=
kubectl create deployment does not have a --replicas option. You could first create it and then scale it using the kubectl scale command.



=
Service
=

1) Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

    kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

    (This will automatically use the pod's labels as selectors)

Or

  kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml  
  (This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)



2) Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:

    kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml

    (This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node  port in manually before creating the service with the pod.)

Or

    kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
