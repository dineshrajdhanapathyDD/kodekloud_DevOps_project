

## Questions:

There are some jobs/tasks that need to be run regularly on different schedules. Currently the Nautilus DevOps team is working on developing some scripts that will be executed on different schedules, but for the time being the team is creating some cron jobs in Kubernetes cluster with some dummy commands (which will be replaced by original scripts later). Create a cronjob as per details given below:

1. Create a cronjob named `xfusion`.

2. Set Its schedule to something like `*/7 * * * *`, you set any schedule for now.

3. Container name should be `cron-xfusion`.

4. Use `nginx` image with `latest tag` only and remember to mention the tag i.e `nginx:latest`.

5. Run a dummy command `echo Welcome to xfusioncorp!`.

6.Ensure restart policy is `OnFailure`.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


## Solution:  

**1.  Kindly check the existing pod & cronjob**

```
thor@jump_host ~$ kubectl get pod
No resources found in default namespace.
thor@jump_host ~$ kubectl get cronjob
No resources found in default namespace.
```

**2.  Create a YAML  file with all the parameters,  Kindly do the changes as per task**

```
thor@jump_host ~$ cat /tmp/cron.yml
apiVersion: batch/v1

kind: CronJob

metadata:

  name: xfusion

spec:

  schedule: "*/7 * * * *"

  jobTemplate:

    spec:

      template:

        spec:

          containers:

            - name: cron-xfusion

              image: nginx:latest

              command:

                - /bin/sh

                - -c

                - echo Welcome to xfusioncorp!

          restartPolicy: OnFailure
```

**3. Run the below command to create a pod**

```
thor@jump_host ~$ kubectl create -f /tmp/cron.yml
cronjob.batch/xfusion created
```

**4.  Wait for the pod to get completed status & check Cronjob**

```
thor@jump_host ~$ kubectl get cronjob
NAME      SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
xfusion   */7 * * * *   False     0        <none>          59s

thor@jump_host ~$  kubectl get namespace
NAME                 STATUS   AGE
default              Active   34m
kube-node-lease      Active   34m
kube-public          Active   34m
kube-system          Active   34m
local-path-storage   Active   34m


thor@jump_host ~$ kubectl get pod -n default
NAME                     READY   STATUS      RESTARTS   AGE
xfusion-28207521-dssnb   0/1     Completed   0          13m
xfusion-28207528-fxc2s   0/1     Completed   0          6m9s


thor@jump_host ~$ kubectl get pod
NAME                     READY   STATUS      RESTARTS   AGE
xfusion-28207521-dssnb   0/1     Completed   0          14m
xfusion-28207528-fxc2s   0/1     Completed   0          7m58s
xfusion-28207535-mhkgn   0/1     Completed   0          58s
```

**5.  Validate the task by running the below command**

```
thor@jump_host ~$ kubectl logs xfusion-28207535-mhkgn
Welcome to xfusioncorp!
```

**6.  Click on `Finish` & `Confirm` to complete the task successfully**





