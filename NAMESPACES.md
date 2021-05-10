# Namespaces
https://livebook.manning.com/book/kubernetes-in-action/chapter-3/224
Namespaces don’t provide any kind of isolation of running objects.
When the solution doesn’t provide inter-namespace network isolation, if a pod in namespace foo knows the IP address of a pod in namespace bar, there is nothing preventing it from sending traffic, such as HTTP requests, to the other pod.

`kubectl get ns`

`kubectl create namespace $NAMESPACE [--dry-run=client]`

`kubectl get po --namespace $NAMESPACE`

`kubectl get po -n $NAMESPACE`

```
apiVersion: v1
kind: Namespace
metadata:
  name: custom-namespace
```

`kubectl create -f kubia-deployment.yaml -n $NAMESPACE`

# Current Context
To quickly switch to a different namespace, you can set up the following alias: 
`alias kcd='kubectl config set-context $(kubectl config current-context) --namespace'`
You can then switch between namespaces using `kcd $NAMESPACE`

Check what your current context is via `kubectl config view | grep namespace`








