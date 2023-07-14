# kube-test

To check the status of the Kubernetes cluster nodes:
- `minikube status`
- `kubectl get nodes`

To delete the nodes:
- `minikube delete`

To start the cluster in debug mode:
- `minikube start --v=7 --alsologtostderr`

To get the number of pods and status of services:
- `kubectl get pod`
- `kubectl get services`

To create a pod deployment (replace `<POD_NAME>` and `<IMAGE_NAME>` with appropriate values):
- `kubectl create deployment <POD_NAME> --image=<IMAGE_NAME>`

Example:
- `kubectl create deployment nginx-depl --image=nginx`

To see the list of deployments, pods, services, and replica sets:
- `kubectl get deployment`
- `kubectl get pod`
- `kubectl get service`
- `kubectl get replicaset`

To edit a deployment:
- `kubectl edit deployment nginx-depl`

To view the logs of a pod:
- `kubectl logs <pod_id>`

Example:
- `kubectl logs nginx-depl-56cb8b6d7-6f66l`

To access the terminal of a pod:
- `kubectl exec -it <pod_id> -- bin/bash`

Example:
- `kubectl exec -it nginx-depl-56cb8b6d7-6f66l -- bin/bash`

To delete a deployment:
- `kubectl delete deployment <deployment_name/id>`

Example:
- `kubectl delete deployment nginx-depl`

To run a deployment using a .yaml file:
- `kubectl apply -f deployment.yaml`

To get the full description of a service:
- `kubectl describe service nginx-service`

To delete a deployment or service:
- `kubectl delete -f nginx-service.yaml`
- `kubectl delete -f nginx-depl.yaml`
