

## Questions:

There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first

1. Create a `Deployment` named as `ic-deploy-nautilus`.

2. Configure `spec` as replicas should be `1`, labels `app` should be `ic-nautilus`, template's metadata lables `app` should be the same `ic-nautilus`.

3.The `initContainers` should be named as `ic-msg-nautilus`, use image `centos`, preferably with `latest` tag and use command `'/bin/bash'`,`'-c'` and `'echo Init Done - Welcome to xFusionCorp Industries > /ic/media'`. The volume mount should be named as `ic-volume-nautilus` and mount path should be `/ic`.


4.Main container should be named as `ic-main-nautilus`, use image `centos`, preferably with `latest` tag and use command `'/bin/bash'`, `'-c'` and `'while true; do cat /ic/media; sleep 5; done'`. The volume mount should be named as `ic-volume-nautilus` and mount path should be `/ic`.


5. Volume to be named as `ic-volume-nautilus` and it should be an emptyDir type.


`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:

**1. Check existing running Pods  & Services**

```

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
thor@jump_host ~$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   15m
thor@jump_host ~$ kubectl get deploy
No resources found in default namespace.
thor@jump_host ~$ 
```

**2.  Create a YAML  file with all the parameters**

```

thor@jump_host ~$ vi /tmp/init.yaml
thor@jump_host ~$ cat /tmp/init.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-nautilus
  labels:
    app: ic-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-nautilus
  template:
    metadata:
      labels:
        app: ic-nautilus
    spec:
      volumes:
        - name: ic-volume-nautilus
          emptyDir: {}
      initContainers:
        - name: ic-msg-nautilus
          image: centos:latest
          command:
            [
              "/bin/bash",
              "-c",
              "echo Init Done - Welcome to xFusionCorp Industries > /ic/media",
            ]
          volumeMounts:
            - name: ic-volume-nautilus
              mountPath: /ic

      containers:
        - name: ic-main-nautilus
          image: centos:latest
          command:
            [
              "/bin/bash",
              "-c",
              "while true; do cat /ic/media; sleep 5; done",
            ]
          volumeMounts:
            - name: ic-volume-nautilus
              mountPath: /ic

thor@jump_host ~$ 
```

**3. Run the below command to create a pod**

```

thor@jump_host ~$ kubectl create -f /tmp/init.yaml
deployment.apps/ic-deploy-nautilus created
```

**4.  Wait for the pod to get completed status**

```

thor@jump_host ~$ kubectl get deploy
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
ic-deploy-nautilus   1/1     1            1           38s
thor@jump_host ~$ kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
ic-deploy-nautilus-7fd4cfcbc7-xpm77   1/1     Running   0          87s
```

**5.  Validate the task by logs & cat the file**

```

thor@jump_host ~$ kubectl logs -f ic-deploy-nautilus-7fd4cfcbc7-xpm77
Defaulted container "ic-main-nautilus" out of: ic-main-nautilus, ic-msg-nautilus (init)
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries

thor@jump_host ~$
thor@jump_host ~$ kubectl exec ic-deploy-nautilus-7fd4cfcbc7-xpm77 -- cat /ic/media
Defaulted container "ic-main-nautilus" out of: ic-main-nautilus, ic-msg-nautilus (init)
Init Done - Welcome to xFusionCorp Industries
```

**6.  Click on `Finish` & `Confirm` to complete the task successfully**
 
