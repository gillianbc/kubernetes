# YAMLs for definining Kubernetes Resources
https://livebook.manning.com/book/kubernetes-in-action/chapter-3/72

## Port
Listing the port the app is listening on is cosmetic.  Others will still be able to access it on its port even if port omitted from the spec.

## Explain
`kubectl api-resources`

`kubectl explain pods`

`kubectl explain pod.spec`

You can drill down and down...
`kubectl explain pod.spec.affinity.podAffinity.preferredDuringSchedulingIgnoredDuringExecution.podAffinityTerm.topologyKey`

## Create a Pod from a Yaml
`kubectl create -f kubia-manual.yaml`
N.B.  `create` will error if the resource already exists, `apply` won't, so `apply` is generally the one to use in CI scripts.

Also see the deployment example.  





