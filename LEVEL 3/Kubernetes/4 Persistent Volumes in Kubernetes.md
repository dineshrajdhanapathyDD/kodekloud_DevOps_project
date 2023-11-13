

## Questions:

The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


1. Create a `PersistentVolume` named as `pv-nautilus`. Configure the spec as storage class should be `manual`, set capacity to `3Gi`, set access mode to `ReadWriteOnce`, volume type should be `hostPath` and set path to `/mnt/dba` (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

2. Create a `PersistentVolumeClaim` named as `pvc-nautilus`. Configure the `spec` as storage class should be `manual`, request `1Gi` of the storage, set access mode to `ReadWriteOnce`.

3. Create a `pod` named as `pod-nautilus`, mount the persistent volume you created with claim name `pvc-nautilus` at document root of the web server, the container within the pod should be named as `container-nautilus` using image `httpd` with `latest` tag only (remember to mention the tag i.e `httpd:latest`).

4. Create a node port type service named `web-nautilus` using node port `30008` to expose the web server running within the pod.

`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:

**1. At first  kubectl  utility configure and working from jump server**

```

thor@jump_host ~$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   18m
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create yaml  file with all the parameters**


```

thor@jump_host ~$ vi httpd.yaml
thor@jump_host ~$ cat httpd.yaml
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nautilus
spec:
  capacity:
    storage: 3Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  hostPath:
    path: /mnt/dba
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nautilus
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  resources:
    requests:
      storage: 1Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-nautilus
  labels:
     app: httpd
spec:
  volumes:
    - name: storage-nautilus
      persistentVolumeClaim:
        claimName: pvc-nautilus
  containers:
    - name: container-nautilus
      image: httpd:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: storage-nautilus
          mountPath:  /usr/local/apache2/htdocs/
---                                                                                                           
apiVersion: v1                                                                                                
kind: Service                                                                                                 
metadata:                                                                                                     
  name: web-nautilus                                                                                         
spec:                                                                                                         
   type: NodePort                                                                                             
   selector:                                                                                                  
     app: httpd                                                                                     
   ports:                                                                                                     
     - port: 80                                                                                               
       targetPort: 80                                                                                         
       nodePort: 30008


thor@jump_host ~$ 
```

**3.  Run below command to create pod**

```

thor@jump_host ~$ kubectl create -f httpd.yaml
persistentvolume/pv-nautilus created
persistentvolumeclaim/pvc-nautilus created
pod/pod-nautilus created
service/web-nautilus created

thor@jump_host ~$ kubectl get all
NAME               READY   STATUS    RESTARTS   AGE
pod/pod-nautilus   1/1     Running   0          51s

NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP        43m
service/web-nautilus   NodePort    10.96.242.131   <none>        80:30008/TCP   51s
```

**4. Make sure pvc & pv are bound status**

```

thor@jump_host ~$ kubectl get pvc
NAME           STATUS   VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nautilus   Bound    pv-nautilus   3Gi        RWO            manual         90s
thor@jump_host ~$ kubectl get pv
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE
pv-nautilus   3Gi        RWO            Retain           Bound    default/pvc-nautilus   manual                  98s
thor@jump_host ~$ 
```

**5.  Wait for  pods & service to get running status**

```

thor@jump_host ~$ kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
pod-nautilus   1/1     Running   0          2m35s


thor@jump_host ~$ kubectl get service
NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP        46m
web-nautilus   NodePort    10.96.242.131   <none>        80:30008/TCP   4m18s
```

**6.  Validate the task by running**
click the website port -> web page shown like -> Index of/ 


**7.  Click on `Finish` & `Confirm` to complete the task successful**