# kube-test

minikube status   //status of nodes of kubernetes cluster

kubectl get nodes //status of nodes of kubernetes cluster

minikube delete //to delete the nodes

minikube start --v=7 --alsologtostderr //start cluster in debug mode

kubectl get pod //to get the number of pod

kubectl get services //to get status of the services

kubectl create deployment <POD_NAME> --image=<IMAGE_NAME>   // to create pod

kubectl create deployment nginx-depl --image=nginx   //if the image is not present it will download the latest iamgees for the docker hub

kubectl get deployment //to see the list of deployment

kubectl get pod // to get the pod status
kubectl get service //to get list of services
kubectl get replicaset  //to see the replicaset status
//replicaset is managing the replicas of a pod
//we never have to CRUD replicaset we directly wor with deployment

//Layers of Abstraction 
//Deployment manages a --> //Replicasset manages a --> //Pod is an abstraction of --> //Container


kubectl edit deployment nginx-depl  //to edit the deployment layer

kubectl logs <pod id (it can be accessed using `kubctl get pod`)>
kubectl logs nginx-depl-56cb8b6d7-6f66l


//TERMINAL
kubectl exec -it <pod id (it can be accessed using `kubctl get pod`)> -- bin/bash
kubectl exec -it nginx-depl-56cb8b6d7-6f66l -- bin/bash


//DELETE
kubectl delete deployment <Deployment name/id (kubectl get deployment)> 

//run deployment using the .yaml file
eg: (for deployment)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-depl
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 80

eg: (for services)
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080

deployment.apps/nginx-depl configured

//full description of service name
kubectl describe service nginx-service
kubectl get pod -o wide
kubectl get deployment nginx-depl -o yaml >nginx-deploment-result.yaml

kubectl delete -f nginx-service.yml/deployment.yaml //will delte the deployment or the service which was started using it

kubectl delete -f nginx-service.yaml
kubectl delete -f nginx-depl.yaml
