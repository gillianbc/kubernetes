# Proxy ~ Kubernetes Dashboard

`kubectl proxy`

To get the token
`kubectl get secrets`
`kubectl describe secret XXXXX`

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

The above worked in Chrome but not Safari

# Minikube Dashboard
`minikube dashboard`

If that doesn't work...
`minikube addons list`

If the dashboard not enabled:
`minikube enable dashboard`

If that hangs on OSX, then do this trick (I googled it, no idea why it works):
`kubectl delete clusterrolebinding kubernetes-dashboard`

Try again:
`minikube enable dashboard`
`minikube dashboard`
Should open dashboard in default browser




