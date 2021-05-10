# Update Labels of a Pod
`kubectl label po kubia-manual-v2 env=debug --overwrite`

# Show Labels
Show labels
`kubectl get po --show-labels`

Show columns for labels creation_method, env and app
`kubectl get po -L creation_method,env,app`

# Get via Label Selector
Pods with a specific label value:
`kubectl get po -l creation_method=manual`

Pods with the following label:
`kubectl get po -l env`

Pods without the following label:
`kubectl get po -l '!env'`

Pods with env label in either prod OR devel
`kubectl get po -l 'env in (prod,devel)' --show-labels`

Pods with env label in neither prod NOR devel (or no env label at all)
`kubectl get po -l 'env notin (prod,devel)' --show-labels`




