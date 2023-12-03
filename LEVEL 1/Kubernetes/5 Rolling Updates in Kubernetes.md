

## Questions:
We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image `nginx:1.17` with the latest changes.

Perform a rolling update for this application and incorporate `nginx:1.17` image. The deployment name is `nginx-deployment`

Make sure all pods are up and running after the update.

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  check existing deployment and  pods running status**

```
thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           104s

thor@jump_host ~$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-989f57c54-2krjf   1/1     Running   0          2m26s
nginx-deployment-989f57c54-hxkb9   1/1     Running   0          2m26s
nginx-deployment-989f57c54-qsggx   1/1     Running   0          2m26s
```

**2.  According the task set the  container image & deployment name**

```
thor@jump_host ~$ kubectl set image deployment nginx-deployment nginx-container=nginx:1.17
deployment.apps/nginx-deployment image updated
```

**3. wait for  deployment and  pods running status**

```
thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-5dd558cf95-542mt   1/1     Running   0          37s
nginx-deployment-5dd558cf95-9sldm   1/1     Running   0          34s
nginx-deployment-5dd558cf95-c5q44   1/1     Running   0          47s
thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           5m59s
thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-5dd558cf95-542mt   1/1     Running   0          2m2s
nginx-deployment-5dd558cf95-9sldm   1/1     Running   0          119s
nginx-deployment-5dd558cf95-c5q44   1/1     Running   0          2m12s
```

**4. Check the rollout deployment status**

```
thor@jump_host ~$ kubectl rollout status deployment nginx-deployment
deployment "nginx-deployment" successfully rolled out
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**

