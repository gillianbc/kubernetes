
# Create
Exercise from:  https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

A deployment is a specification of what you want kubernetes to provide.  
Via the yaml, we've asked for 3 nginx instances based on nginx:1.14.2 listening on port 80

`kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml`
or seeing as I have it locally too:
`kubectl apply -f deployment.yaml`

If you have multiple controllers that have overlapping selectors, the controllers will fight with each other and won't behave correctly.

`kubectl get deployments`

`kubectl rollout status deployment/nginx-deployment`

`kubectl get pods --show-labels`
You'll see the labels have been applied as per the deployment specification

# Update
Let's update the image of nginx and let kubernetes roll it out.  Using --record will save the details of the change in case we need to examine it later

? I don't understand this syntax.  Why not just `nginx-deployment`?

`kubectl --record deployment.apps/nginx-deployment set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1`

OR

`kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1 --record`

OR

`kubectl edit deployment.v1.apps/nginx-deployment`
(will open in vi)

`kubectl rollout status deployment/nginx-deployment `

`kubectl describe deployment nginx-deployment`

`kubectl get rs`
There will be 2 replica sets - the original which will cause the old pods to scale down to 0 and the new set which will cause the scale up of the new pods.


```gillianbc@Gillians-MacBook deployment-example % kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           3m57s
gillianbc@Gillians-MacBook deployment-example % kubectl get rs         
NAME                          DESIRED   CURRENT   READY   AGE
kubia-rs                      2         2         2       13h
nginx-deployment-559d658b74   3         3         3       3m11s
nginx-deployment-66b6c48dd5   0         0         0       4m1s
gillianbc@Gillians-MacBook deployment-example % kubectl get pods       
NAME                                READY   STATUS    RESTARTS   AGE
kubia                               1/1     Running   0          21h
kubia-rs-58jgp                      1/1     Running   0          13h
kubia-rs-zxltc                      1/1     Running   0          13h
nginx-deployment-559d658b74-9bf6g   1/1     Running   0          3m7s
nginx-deployment-559d658b74-b9mv4   1/1     Running   0          3m15s
nginx-deployment-559d658b74-btl7k   1/1     Running   0          3m12s
```

You can see that the pod names are suffixed with the hash of the new replica set: `559d658b74`

# Rollback a Deployment
? I don't understand this syntax.  Why not just `nginx-deployment`?
`kubectl rollout history deployment.v1.apps/nginx-deployment`

`kubectl rollout undo deployment.v1.apps/nginx-deployment`
OR
`kubectl rollout undo deployment.v1.apps/nginx-deployment --to-revision=2`

# Scale a Deployment Up or Down
`kubectl scale deployment.v1.apps/nginx-deployment --replicas=10`



