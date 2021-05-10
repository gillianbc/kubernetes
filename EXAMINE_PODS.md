# EXAMINING PODS
I tend to set $POD_NAME to save typing e.g. 
`POD_NAME=kubia-deployment-c75b676fc-4npmh`

## Get Details of Pod
NB.  po = pod shorthand
`kubectl get po kubia-manual`
`kubectl get po kubia-manual -o wide`
`kubectl get po kubia-manual -o yaml`
`kubectl get po kubia-manual -o json`

Show labels
`kubectl get po --show-labels`

Show columns for labels creation_method, env and app
`kubectl get po -L creation_method,env,app`

## Get Container Logs
We're using a docker image in the container so if we ssh'd into the container, we could do:
`docker logs <container id>`   

We can get the container id via `kubectl describe $POD_NAME`, but I don't yet know how to ssh into the container
`kubectl logs $POD_NAME` 

If there are multiple containers in the pod (discouraged), then:
`kubectl logs $POD_NAME -c <container name`

## Port Forwarding
The following command will forward your machine’s local port 8888 to port 8080 of your pod:

`kubectl port-forward $POD_NAME 8888:8080`

Check it:
`curl localhost:8888`