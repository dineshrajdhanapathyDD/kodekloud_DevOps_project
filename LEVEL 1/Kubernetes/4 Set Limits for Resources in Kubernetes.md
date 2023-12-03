

## Questions:
Recently some of the performance issues were observed with some applications hosted on Kubernetes cluster. The Nautilus DevOps team has observed some resources constraints, where some of the applications are running out of resources like memory, cpu etc., and some of the applications are consuming more resources than needed. Therefore, the team has decided to add some limits for resources utilization. Below you can find more details.


Create a pod named `httpd-pod` and a container under it named as `httpd-container`, use `httpd` image with `latest` tag only and remember to mention tag i.e `httpd:latest` and set the following limits:

Requests: Memory: `15Mi`, CPU: `100m`

Limits: Memory: `20Mi`, CPU: `100m`

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```
thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   28m
kube-node-lease      Active   28m
kube-public          Active   28m
kube-system          Active   28m
local-path-storage   Active   28m
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create yaml  file with all the parameters , you can copy form gitlab**
    
```
thor@jump_host ~$ vi /tmp/limit.yaml
thor@jump_host ~$ cat /tmp/limit.yaml
apiVersion: v1

kind: Pod

metadata:

  name: httpd-pod

spec:

  containers:

  - name: httpd-container

    image: httpd:latest

    resources:

      requests:

        memory: "15Mi"

        cpu: "100m"

      limits:

        memory: "20Mi"

        cpu: "100m"
```

**3.  Run below command to create pod**

```
thor@jump_host ~$ kubectl create -f /tmp/limit.yaml
pod/httpd-pod created
```

**4.  Wait for  pods to get running status**  

```
thor@jump_host ~$ kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          39s
```

**5.  Validate the task by running**

```
thor@jump_host ~$ kubectl describe pods httpd-pod
Name:             httpd-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Tue, 15 Aug 2023 05:59:12 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  httpd-container:
    Container ID:   containerd://48e0ee1ab09cf8b37ccef38a7aabc383a054edafa0ba4a6031c1e0762cb36f8b
    Image:          httpd:latest
    Image ID:       docker.io/library/httpd@sha256:d7262c0f29a26349d6af45199b2770d499c74d45cee5c47995a1ebb336093088
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 15 Aug 2023 05:59:21 +0000
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     100m
      memory:  20Mi
    Requests:
      cpu:        100m
      memory:     15Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-5gvxp (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-5gvxp:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  73s   default-scheduler  Successfully assigned default/httpd-pod to kodekloud-control-plane
  Normal  Pulling    72s   kubelet            Pulling image "httpd:latest"
  Normal  Pulled     65s   kubelet            Successfully pulled image "httpd:latest" in 6.858696646s (6.85871279s including waiting)
  Normal  Created    65s   kubelet            Created container httpd-container
  Normal  Started    64s   kubelet            Started container httpd-container
```

**6.  Click on `Finish` & `Confirm` to complete the task successful**

