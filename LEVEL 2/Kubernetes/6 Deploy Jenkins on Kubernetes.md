

## Questions:

The Nautilus DevOps team is planning to set up a Jenkins CI server to create/manage some deployment pipelines for some of the projects. They want to set up the Jenkins server on Kubernetes cluster. Below you can find more details about the task:


1) Create a namespace `jenkins`

2) Create a Service for jenkins deployment. Service name should be `jenkins-service` under `jenkins` namespace, type should be `NodePort`, nodePort should be `30008`

3) Create a Jenkins Deployment under `jenkins` namespace, It should be name as `jenkins-deployment` , labels `app` should be `jenkins` , container name should be `jenkins-container` , use `jenkins/jenkins` image , containerPort should be 8080 and replicas count should be `1`.


Make sure to wait for the pods to be in running state and make sure you are able to access the Jenkins login screen in the browser before hitting the `Check` button.


`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:

**1. At first  kubectl  utility configure and working from jump server, run below commands**   

```

thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   20m
kube-node-lease      Active   20m
kube-public          Active   20m
kube-system          Active   20m
local-path-storage   Active   20m


thor@jump_host ~$ kubectl get service
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   20m
```

**2.  Create namespace as per the task request.**

```

thor@jump_host ~$ kubectl create namespace  jenkins
namespace/jenkins created


thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   21m
jenkins              Active   18s
kube-node-lease      Active   21m
kube-public          Active   21m
kube-system          Active   21m
local-path-storage   Active   21m
```

**3.  Create  jenkins yaml  file with all the parameters** 

```

thor@jump_host ~$ vi /tmp/jenkins.yaml
thor@jump_host ~$ cat /tmp/jenkins.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: jenkins
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
  labels:
    app: jenkins
  namespace: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      name: jenkins
      labels:
        app: jenkins
    spec:
      containers:
        - name: jenkins-container
          image: jenkins/jenkins
          ports:
            - containerPort: 8080
          imagePullPolicy: IfNotPresent
      restartPolicy: Always
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: jenkins
spec:
  selector:
    app: jenkins
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30008
  type: NodePort
```

**4.  Run the below command to create a pod**

```

thor@jump_host ~$ kubectl create -f /tmp/jenkins.yaml
deployment.apps/jenkins-deployment created
service/nginx-service created
Error from server (AlreadyExists): error when creating "/tmp/jenkins.yaml": namespaces "jenkins" already exists
```

**5. Wait for deployment & pods to get running status**

```

thor@jump_host ~$ kubectl get deploy -n jenkins
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
jenkins-deployment   1/1     1            1           68s


thor@jump_host ~$ kubectl get pods -n jenkins
NAME                                  READY   STATUS    RESTARTS   AGE
jenkins-deployment-848875ffc5-bpblx   1/1     Running   0          108s

thor@jump_host ~$ kubectl get service  -n jenkins
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   10.96.209.188   <none>        80:30008/TCP   2m17s
```

**6. Validate the task by curl or open the browser by clicking `'Open Port on Host 1'`**

```

thor@jump_host ~$ kubectl exec jenkins-deployment-848875ffc5-bpblx -n jenkins -- curl http://localhost:8080
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   541  100   541    0     0     36      0  0:00:15  0:00:15 --:--:--   163
<html><head><meta http-equiv='refresh' content='1;url=/login?from=%2F'/><script>window.location.replace('/login?from=%2F');</script></head><body style='background-color:white; color:white;'>


Authentication required
<!--
-->

</body></html>                                                                                                                                                                                                                                                                                                        7. Give password for user admin to login jenkins:

thor@jump_host ~$ kubectl exec jenkins-deployment-848875ffc5-bpblx -n jenkins -- cat /var/jenkins_home/secrets/initialAdminPassword
0da201cec4fc47e68a55911ebcffd548  


thor@jump_host ~$ kubectl exec jenkins-deployment-848875ffc5-bpblx -n jenkins -- curl --user admin:0da201cec4fc47e68a55911ebcffd548 http://localhost:8080/
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:05 --:--:--     0
  
  <!DOCTYPE html><html><head resURL="/static/cd0a90e3" data-rooturl="" data-resurl="/static/cd0a90e3" data-extensions-available="true" data-unit-test="false" data-imagesurl="/static/cd0a90e3/images" data-crumb-header="Jenkins-Crumb" data-crumb-value="79ffbc6f250df0f77295fc0eb0d87b94527b167a4001e0c01e7b87fc13024030">
    
    

    <title>Setup Wizard [Jenkins]</title><link rel="stylesheet" href="/static/cd0a90e3/jsbundles/styles.css" type="text/css"><link rel="stylesheet" href="/static/cd0a90e3/css/responsive-grid.css" type="text/css"><link rel="icon" href="/static/cd0a90e3/favicon.svg" type="image/svg+xml"><link sizes="any" rel="alternate icon" href="/static/cd0a90e3/favicon.ico"><link sizes="180x180" rel="apple-touch-icon" href="/static/cd0a90e3/apple-touch-icon.png"><link color="#191717" rel="mask-icon" href="/static/cd0a90e3/mask-icon.svg"><meta name="theme-color" content="#ffffff"><script src="/static/cd0a90e3/scripts/prototype.js" type="text/javascript"></script><script src="/static/cd0a90e3/scripts/behavior.js" type="text/javascript"></script><script src='/adjuncts/cd0a90e3/org/kohsuke/stapler/bind.js' type='text/javascript'></script><script src="/static/cd0a90e3/scripts/yui/yahoo/yahoo-min.js"></script><script src="/static/cd0a90e3/scripts/yui/dom/dom-min.js"></script><script src="/static/cd0a90e3/scripts/yui/event/event-min.js"></script><script src="/static/cd0a90e3/scripts/yui/animation/animation-min.js"></script><script src="/static/cd0a90e3/scripts/yui/dragdrop/dragdrop-min.js"></script><script src="/static/cd0a90e3/scripts/yui/container/container-min.js"></script><script src="/static/cd0a90e3/scripts/yui/connection/connection-min.js"></script><script src="/static/cd0a90e3/scripts/yui/datasource/datasource-min.js"></script><script src="/static/cd0a90e3/scripts/yui/autocomplete/autocomplete-min.js"></script><script src="/static/cd0a90e3/scripts/yui/menu/menu-min.js"></script><script src="/static/cd0a90e3/scripts/yui/element/element-min.js"></script><script src="/static/cd0a90e3/scripts/yui/button/button-min.js"></script><script src="/static/cd0a90e3/scripts/yui/storage/storage-min.js"></script><script src="/static/cd0a90e3/scripts/hudson-behavior.js" type="text/javascript"></script><script src="/static/cd0a90e3/scripts/sortable.js" type="text/javascript"></script><link rel="stylesheet" href="/static/cd0a90e3/scripts/yui/container/assets/container.css" type="text/css"><link rel="stylesheet" href="/static/cd0a90e3/scripts/yui/container/assets/skins/sam/container.css" type="text/css"><link rel="stylesheet" href="/static/cd0a90e3/scripts/yui/menu/assets/skins/sam/menu.css" type="text/css"><link rel="search" href="/opensearch.xml" type="application/opensearchdescription+xml" title="Jenkins"><meta name="ROBOTS" content="INDEX,NOFOLLOW"><meta name="viewport" content="width=device-width, initial-scale=1"><script src="/static/cd0a90e3/jsbundles/vendors.js" type="text/javascript"></script><script src="/static/cd0a90e3/jsbundles/sortable-drag-drop.js" type="text/javascript"></script><script defer="true" src="/static/cd0a90e3/jsbundles/app.js" type="text/javascript"></script></head><body data-model-type="jenkins.install.SetupWizard" id="jenkins" class="yui-skin-sam full-screen jenkins-2.423" data-version="2.423"><div id="page-body" class="app-page-body app-page-body--full-screen clear"><div id="main-panel"><a id="skip2content"></a><div class="plugin-setup-wizard-container"></div><div id="default-site-id"></div><script src='/adjuncts/cd0a90e3/jenkins/install/SetupWizard/_wizard-ui.js' type='text/javascript'></script><script src="/static/cd0a90e3/jsbundles/pluginSetupWizard.js" type="text/javascript"></script><link rel="stylesheet" href="/static/cd0a90e3/jsbundles/pluginSetupWizard.c100  3799  100  3799    0     0    684      0  0:00:05  0:00:05 --:--:--   872
```

**8.  Click on `Finish` & `Confirm` to complete the task successful** 


