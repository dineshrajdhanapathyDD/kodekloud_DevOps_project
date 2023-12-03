

## Questions:

This morning the Nautilus DevOps team rolled out a new release for one of the applications. Recently one of the customers logged a complaint which seems to be about a bug related to the recent release. Therefore, the team wants to rollback the recent release.

There is a deployment named `nginx-deployment`; roll it back to the previous revision.

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution : 

**1. At first  check existing deployment and pods running status**  

```
thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           89s

thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-665bf769f5-6qm4h   1/1     Running   0          90s
nginx-deployment-665bf769f5-hmpz2   1/1     Running   0          87s
nginx-deployment-665bf769f5-z64xz   1/1     Running   0          99s


thor@jump_host ~$ kubectl describe deploy nginx-deployment
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Thu, 17 Aug 2023 15:13:12 +0000
Labels:                 app=nginx-app
                        type=front-end
Annotations:            deployment.kubernetes.io/revision: 2
                        kubernetes.io/change-cause:
                          kubectl set image deployment nginx-deployment nginx-container=nginx:alpine --kubeconfig=/root/.kube/config --record=true
Selector:               app=nginx-app
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx-app
  Containers:
   nginx-container:
    Image:        nginx:alpine
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  nginx-deployment-989f57c54 (0/0 replicas created)
NewReplicaSet:   nginx-deployment-698959d995 (3/3 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  94s   deployment-controller  Scaled up replica set nginx-deployment-989f57c54 to 3
  Normal  ScalingReplicaSet  83s   deployment-controller  Scaled up replica set nginx-deployment-698959d995 to 1
  Normal  ScalingReplicaSet  74s   deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 2 from 3
  Normal  ScalingReplicaSet  74s   deployment-controller  Scaled up replica set nginx-deployment-698959d995 to 2 from 1
  Normal  ScalingReplicaSet  71s   deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 1 from 2
  Normal  ScalingReplicaSet  71s   deployment-controller  Scaled up replica set nginx-deployment-698959d995 to 3 from 2
  Normal  ScalingReplicaSet  68s   deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 0 from 1
```

**2. Run the below command to revert to an earlier release**

```
thor@jump_host ~$ kubectl rollout undo deployment  nginx-deployment
deployment.apps/nginx-deployment rolled back
```

**3. Wait for deployment & pods to get running status**

```
thor@jump_host ~$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-989f57c54-6q47v   1/1     Running   0          9s
nginx-deployment-989f57c54-gf4fm   1/1     Running   0          11s
nginx-deployment-989f57c54-jxmqq   1/1     Running   0          5s

thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           5m17s
thor@jump_host ~$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-989f57c54-6q47v   1/1     Running   0          3m20s
nginx-deployment-989f57c54-gf4fm   1/1     Running   0          3m22s
nginx-deployment-989f57c54-jxmqq   1/1     Running   0          3m16s
```

**4. Validate the task by running the below command**

```
thor@jump_host ~$ kubectl rollout status deployment  nginx-deployment
deployment "nginx-deployment" successfully rolled out
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**




error shown:
Release has not been rolled back

check the container image like " Image:        nginx:alpine" 
if not error found

thor@jump_host ~$ kubectl describe deploy nginx-deployment
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Thu, 17 Aug 2023 12:17:32 +0000
Labels:                 app=nginx-app
                        type=front-end
Annotations:            deployment.kubernetes.io/revision: 3
Selector:               app=nginx-app
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx-app
  Containers:
   nginx-container:
    Image:        nginx:1.16
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  nginx-deployment-698959d995 (0/0 replicas created)
NewReplicaSet:   nginx-deployment-989f57c54 (3/3 replicas created)
Events:
  Type    Reason             Age                  From                   Message
  ----    ------             ----                 ----                   -------
  Normal  ScalingReplicaSet  2m38s                deployment-controller  Scaled up replica set nginx-deployment-989f57c54 to 3
  Normal  ScalingReplicaSet  2m28s                deployment-controller  Scaled up replica set nginx-deployment-698959d995 to 1
  Normal  ScalingReplicaSet  2m20s                deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 2 from 3
  Normal  ScalingReplicaSet  2m20s                deployment-controller  Scaled up replica set nginx-deployment-698959d995 to 2 from 1
  Normal  ScalingReplicaSet  2m17s                deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 1 from 2
  Normal  ScalingReplicaSet  2m17s                deployment-controller  Scaled up replica set nginx-deployment-698959d995 to 3 from 2
  Normal  ScalingReplicaSet  2m14s                deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 0 from 1
  Normal  ScalingReplicaSet  107s                 deployment-controller  Scaled up replica set nginx-deployment-989f57c54 to 1 from 0
  Normal  ScalingReplicaSet  105s                 deployment-controller  Scaled down replica set nginx-deployment-698959d995 to 2 from 3
  Normal  ScalingReplicaSet  100s (x4 over 105s)  deployment-controller  (combined from similar events): Scaled down replica set nginx-deployment-698959d995 to 0 from 1











 