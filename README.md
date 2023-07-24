# kube-test

To check the status of the Kubernetes cluster nodes:
- `minikube status` (status of nodes of the Kubernetes cluster)
- `kubectl get nodes` (status of nodes of the Kubernetes cluster)

To watch Pods staus
- kubectl get pods -w

#### When we create a Deployment it create a Replicaset which will create a Pod
- Deployment >> Replicaset >> Pod

To delete the nodes:
- `minikube delete` (to delete the nodes)

To start the cluster in debug mode:
- `minikube start --v=7 --alsologtostderr` (start cluster in debug mode)

To get the number of pods and status of services:
- `kubectl get pod` (to get the number of pods)
- `kubectl get services` (to get the status of the services)

To create a pod deployment (replace `<POD_NAME>` and `<IMAGE_NAME>` with appropriate values):
- `kubectl create deployment <POD_NAME> --image=<IMAGE_NAME>` (to create a pod)

Example:
- `kubectl create deployment nginx-depl --image=nginx` (if the image is not present, it will download the latest image from Docker Hub)

To see the list of deployments, pods, services, and replica sets:
- `kubectl get deployment` (to see the list of deployments)
- `kubectl get pod` (to get the pod status)
- `kubectl get service` (to get the list of services)
- `kubectl get replicaset` (to see the replica set status)

Layers of Abstraction:
Deployment manages a -> ReplicaSet manages a -> Pod is an abstraction of -> Container

To edit a deployment:
- `kubectl edit deployment nginx-depl` (to edit the deployment layer)

To view the logs of a pod:
- `kubectl logs <pod_id>` (it can be accessed using `kubectl get pod`)

Example:
- `kubectl logs nginx-depl-56cb8b6d7-6f66l`

To access the terminal of a pod:
- `kubectl exec -it <pod_id> -- bin/bash` (it can be accessed using `kubectl get pod`)

Example:
- `kubectl exec -it nginx-depl-56cb8b6d7-6f66l -- bin/bash`

To delete a deployment:
- `kubectl delete deployment <deployment_name/id>` (kubectl get deployment)

Example:
- `kubectl delete deployment nginx-depl`

To run a deployment using a .yaml file, you can use the following YAML examples:

For deployment:
```yaml
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
```

For services:
```yaml
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
```

## kubectl apply -f <filename>.yaml

To get the full description of a service:
- `kubectl describe service nginx-service` (full description of service name)

To get the service url
- minikube service `<SERVICE_NAME>` --url

To delete a deployment or service:

Delete the deployment or service using the following command:
- `kubectl delete -f nginx-service.yml` (to delete the service)
- `kubectl delete -f nginx-depl.yaml` (to delete the deployment)

Enable kubernetes ingress and dashboard
- minikube addons enable/disable ingress
- minikube addons enable dashboard

Command for kube-system pods
- kubectl get pod -n kube-system

Kubernetes-dashoard service, deployment, pod, replicaset details
- kubectl get all -n kubernetes-dashboard

To watch the ingress creating process add --watch
- kubectl get ingress -n kubernetes-dashboard --watch

## For creating a local custom domain

kubectl get all -n kubernetes-dashboard
```
NAME                                            READY   STATUS    RESTARTS   AGE
pod/dashboard-metrics-scraper-5c6664855-668km   1/1     Running   0          52m
pod/kubernetes-dashboard-55c4cbbc7c-8dlbn       1/1     Running   0          52m

NAME                                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/dashboard-metrics-scraper   ClusterIP   10.101.96.71     <none>        8000/TCP   52m
service/kubernetes-dashboard        ClusterIP   10.100.190.247   <none>        80/TCP     52m

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dashboard-metrics-scraper   1/1     1            1           52m
deployment.apps/kubernetes-dashboard        1/1     1            1           52m

NAME                                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/dashboard-metrics-scraper-5c6664855   1         1         1       52m
replicaset.apps/kubernetes-dashboard-55c4cbbc7c       1         1         1       52m
```

```
rajat@Rajat--Laptop:/$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	Rajat--Laptop

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

192.168.49.2 dashboard.com
```

To access the termial of the containers running inside the pod
- kubectl exec -it nginx -c sidecar -- /bin/sh
- kubectl exec -it <podname> -c <image name specified int the .yaml file> -- /bin/sh

To list all the docker container running in minikube
- (you will see all the containers which are running inside the minikube as all the contaners are not visible by default)
- eval $(minikube docker-env)
- docker ps 

To use the persistent Volume in kubernetes create a Persisten Volume Claim file
ed:

```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-name
spec:
  storageClassName: manual
  volumeMode: FileSystem
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

To use it add the `volume` field in the pod with the name mapping to the file name
eg:

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      prsistentVolumeClaim:
        claimName: pvc-name
```
#### Claims must present in the same name spaces in the cluster in which the pod exist

#### Configmp and Secret
- `kubectl get configmap` (to get the configed files info)
- local volumes (both)
- not created via PV(persistent volume) or PVC (persistent volume claim)
- managed by kubernetes itself
- ( consider a case where you need a cofig file for prometheus pod or massage broker like mosquito or consider when you needed certificated file mounted inside your application in both cases you need file avaialbe to a pod)
- in these cases the config and secret file and you can mount that into the pod same as PVC
- same as PVC volume it is request and claimed by pvc
- therefore in order to access it in Persistent volume claim of PVC 

#### Storage Class
- creates or provisions Persistent Volumes dynamically in the background
- can be created and configured using .yaml file
- provisioner fields tells kubernetes which provisioner to be used for specific storage platform or cloud provider to create a persistant storage out of it
- so when pod claims storage through PVC, PVC will request storage from storage class, which then will provision create persistent volume that meets the needs of that claim using the provisioner from actual storage backend

#### storageclass.yaml
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: storage-class-name
provisioner: kubernetes.io/aws-ebs
parameters:
  type: io1
  iopsPerGB: "10"
  fsType: ext4
```
#### persistentVolumeClaim.yaml
```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-name
spec:
  storageClassName: manual
  volumeMode: FileSystem
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: storage-class-name
```
- here the PVC is using the storageclassname field to map to the storage class
