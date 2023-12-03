

## Questions:

The Nautilus DevOps team has started practicing some pods, and services deployment on Kubernetes platform, as they are planning to migrate most of their applications on Kubernetes. Recently one of the team members has been assigned a task to create a deployment as per details mentioned below:


Create a deployment named `httpd` to deploy the application `httpd` using the image `httpd:latest` (remember to mention the tag as well)

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```
thor@jump_host ~$ kubectl get deploy
No resources found in default namespace.

thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   9m44s
kube-node-lease      Active   9m44s
kube-public          Active   9m44s
kube-system          Active   9m44s
local-path-storage   Active   9m36s
```

**2.  Create deploy & and run image as per the task request**

```
thor@jump_host ~$ kubectl create deploy httpd --image httpd:latest
deployment.apps/httpd created
```

**3.  Validate the task by running below commands**

```
thor@jump_host ~$ kubectl get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
httpd   1/1     1            1           27s

thor@jump_host ~$ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
httpd-69545969bd-d6vx7   1/1     Running   0          51s

thor@jump_host ~$ kubectl describe pod httpd-69545969bd-d6vx7
Name:             httpd-69545969bd-d6vx7
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Mon, 14 Aug 2023 09:52:00 +0000
Labels:           app=httpd
                  pod-template-hash=69545969bd
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:           10.244.0.5
Controlled By:  ReplicaSet/httpd-69545969bd
Containers:
  httpd:
    Container ID:   containerd://210ff7310924847a13a697b16e99b2dcfdcaf862fa5852aa05d90e8dc21f6df6
    Image:          httpd:latest
    Image ID:       docker.io/library/httpd@sha256:d7262c0f29a26349d6af45199b2770d499c74d45cee5c47995a1ebb336093088
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Mon, 14 Aug 2023 09:52:16 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-mw8mk (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-mw8mk:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  91s   default-scheduler  Successfully assigned default/httpd-69545969bd-d6vx7 to kodekloud-control-plane
  Normal  Pulling    91s   kubelet            Pulling image "httpd:latest"
  Normal  Pulled     76s   kubelet            Successfully pulled image "httpd:latest" in 14.881488286s (14.881506619s including waiting)
  Normal  Created    76s   kubelet            Created container httpd
  Normal  Started    75s   kubelet            Started container httpd
```

**4.  Click on `Finish` & `Confirm` to complete the task successful**
