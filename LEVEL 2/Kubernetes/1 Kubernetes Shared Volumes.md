

## Questions:

We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

1. Create a pod named `volume-share-datacenter`.

2. For the first container, use image `ubuntu` with `latest` tag only and remember to mention the tag i.e `ubuntu:latest`, container should be named as `volume-container-datacenter-1`, and run a `sleep` command for it so that it remains in running state. Volume `volume-share` should be mounted at path `/tmp/official`.

3. For the second container, use image `ubuntu` with the `latest` tag only and remember to mention the tag i.e `ubuntu:latest`, container should be named as `volume-container-datacenter-2`, and again run a `sleep` command for it so that it remains in running state. Volume `volume-share` should be mounted at path `/tmp/games`.

4. Volume name should be `volume-share` of type `emptyDir`.

5.After creating the pod, exec into the first container i.e `volume-container-datacenter-1`, and just for testing create a file `official.txt` with any content under the mounted path of first container i.e `/tmp/official`.


6. The file `official.txt` should be present under the mounted path `/tmp/games` on the second container `volume-container-datacenter-2` as well, since they are using a shared volume.


`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```

thor@jump_host ~$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   12m
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create yaml  file with all the parameters**

```

thor@jump_host ~$ vi /tmp/volume.yaml
thor@jump_host ~$ cat /tmp/volume.yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-nautilus
spec:
  containers:
    - name: volume-container-nautilus-1
      image: ubuntu:latest
      imagePullPolicy: IfNotPresent
      command:
        - /bin/sh
        - -c
      args:
        - while true; do sleep 10; done
      volumeMounts:
        - mountPath: /tmp/news
          name: volume-share
    - name: volume-container-nautilus-2
      image: ubuntu:latest
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/games
      command:
        - /bin/sh
        - -c
      args:
        - while true; do sleep 10; done
  volumes:
    - name: volume-share
      emptyDir: {}
  restartPolicy: Always
```

**3.  Run below command to create pod**

```
thor@jump_host ~$ kubectl create -f /tmp/volume.yaml
pod/volume-share-nautilus created
```

**4.  Wait for  pods to get running status**

```

thor@jump_host ~$ kubectl get pods
NAME                    READY   STATUS    RESTARTS   AGE
volume-share-nautilus   2/2     Running   0          18s



thor@jump_host ~$ kubectl get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE                      NOMINATED NODE   READINESS GATES
volume-share-nautilus   2/2     Running   0          2m31s   10.244.0.5   kodekloud-control-plane   <none>           <none>
```

**5. Create a file `official.txt` with any content under the mounted path of first container i.e `/tmp/official`.**

```

thor@jump_host ~$ kubectl exec -it volume-share-nautilus -c volume-container-nautilus-1 -- /bin/sh
# 
# echo "Welcome to Xfusioncorp Industries" > /tmp/news/news.txt
# cat /tmp/news/news.txt
Welcome to Xfusioncorp Industries
# exit
```

**6. Create a shared volume using the file `official.txt` should be present under the mounted path `/tmp/games` on the second container `volume-container-datacenter-2` as well,**

```

thor@jump_host ~$ kubectl exec -it volume-share-nautilus -c volume-container-nautilus-2 -- ls /tmp/games
news.txt
```

**5.  Click on `Finish` & `Confirm` to complete the task successful.**
