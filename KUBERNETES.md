# 2.2 Kubernetes in Action - 2.2.1
https://livebook.manning.com/book/kubernetes-in-action?origin=dashboard

## Get Minikube Up and Running a Single Node Cluster
`brew install minikube`

`minikube start`

`kubectl cluster-info`

`kubectl describe node <nodename>   `  
e.g. minikube

# Run an image in Kubernetes where I pushed the image to Docker Hub 
`kubectl run kubia --image=gillianbc/kubia --port=8080`  

`kubectl get pods`
`kubectl describe pods`
`kubectl describe pod kubia`

Note:  best not to use 'run' to create pods directly;  use a *deployment*

## Listing 3.7 Creating a three-node cluster in GKE

`gcloud config set compute/zone europe-west3-c`
`gcloud container clusters create kubia --num-nodes 3`
THIS GAVE ERROR 500

## Create Service for Accessing the app running in the pod
`kubectl expose pod kubia --type=LoadBalancer --name=my-service`

`kubectl get services`
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP          7h53m
my-service   LoadBalancer   10.111.243.171   <pending>     8080:32234/TCP   20m

Will take a minute for the external IP address to be exposed (if cluster is local minikube, then it will forever say pending).
You can then access the service on <external-ip>:<port> from anywhere in the world!

### Minikube Service
On minkube, you will not get an external ip so to access service my-service

`minikube service my-service`

This will open up the browser and I'll see my kubia app has printed 'You've hit host kubia'

gillianbc@Gillians-MacBook Versions % minikube service my-service
|-----------|------------|-------------|---------------------------|
| NAMESPACE |    NAME    | TARGET PORT |            URL            |
|-----------|------------|-------------|---------------------------|
| default   | my-service |        8080 | http://192.168.49.2:32234 |
|-----------|------------|-------------|---------------------------|
🏃  Starting tunnel for service my-service.
|-----------|------------|-------------|------------------------|
| NAMESPACE |    NAME    | TARGET PORT |          URL           |
|-----------|------------|-------------|------------------------|
| default   | my-service |             | http://127.0.0.1:56751 |
|-----------|------------|-------------|------------------------|


## Creating a Replica Set Using a Yaml (Don't do this)
e.g. replic.yml is:
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: kubia
  labels:
    app: gillian1
    tier: kubia
spec:
  # modify replicas according to your case
  replicas: 2
  selector:
    matchLabels:
      tier: kubia
  template:
    metadata:
      labels:
        tier: kubia
    spec:
      containers:
      - name: kubia
        image: gillianbc/kubia
        ports:
        - containerPort: 8080
```

`kubectl apply -f replic.yml`

`kubect get pods`

## Creating a Service to Access a Pod

`kubectl expose pod kubia2-jp74g --type=LoadBalancer --name=my-service2`

`kubectl get services`

Port
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
Note: A Service can map any incoming port to a targetPort. By default and for convenience, the targetPort is set to the same value as the port field.	   i.e. port 80 means an external port of 80 referencing something that uses an internal port of 80.   
port: 80 targetPort: 9376 means external port 80 and internal port 9376

## Run the Service on Minikube
minikube service my-service2

`kubectl describe services`
`kubectl describe rs `            (rs means replica sets)

## Get a List of All the Things You can List with 'Get'
`kubectl api-resources`

## Don't Use Replica Sets (nor the old replication controllers)
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
A ReplicaSet ensures that a specified number of pod replicas are running at any given time. However, a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features. Therefore, we recommend using Deployments instead of directly using ReplicaSets, unless you require custom update orchestration or don't require updates at all.
This actually means that you may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.


