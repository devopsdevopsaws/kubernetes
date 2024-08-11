## Install the eks cluster using the below git hub link and also make sure that u create IAM user and do aws configure to work
```
https://github.com/devopsdevopsaws/k8-install/blob/master/README.md
```

## Some basic kuberneteds commands are:
```
kubectl get nodes --> To give the node information 
kubectl describe node <node-name> to describe the node 
kubectl get pods  --> to check pod information
kubectl get pods -A --> to dispaly all wode
kubectl get pods -o wide --> to give pods wide information like ip adress and all
kubectl get ns --> to get name space infortamtion 
kubectl apply -f <>.yaml --> create a pod or any k8s resources using this command
kubectl exec -it <pod-name> -- bash --> u can login to container
kubectl describe pod <pod-name>
kubcetl logs -f <pod-name>

## PODS

kubectl get pods
kubectl get pods -A
kubectl get pods -o wide
kubectl describe pod <podname>
kubectl logs -f <podname>
kubectl exec -it <podname> -- bash
kubectl delete pod <podname>

## SVC

kubectl get svc
kubectl describe svc <svc_name>
kubectl delete svc <svc_name>

## nodes

kubectl get nodes
kubectl describe node <nodename>
kubectl delete node <nodename>

## Secret

kubectl get secret
kubectl describe secret <secretname>
kubectl delete secret <secretname>

## configmaps

kubectl get configmap
kubectl describe configmap <configmapname>
kubectl delete configmap <configmapname>

## Replciaset

kubectl get rs
kubectl describe rs <rsname>
kubectl delete rs <rsname>

## Delpoyment
kubectl get deployment
kubectl describe deployment <deploymentname>
kubectl delete deployment <deploymentname>
```
### COVER below concepts of kubernetes 
*before going to realtime project*
*Refer Hand book frst*
```
https://github.com/devopsdevopsaws/kubernetes/tree/master/k8s_resources

-rw-rw-r-- 1 ec2-user ec2-user  59 Jul 28 07:36 01-namespace.yaml
-rw-rw-r-- 1 ec2-user ec2-user 161 Jul 28 07:36 02-pod.yaml
-rw-rw-r-- 1 ec2-user ec2-user 216 Jul 28 07:40 03-multipod.yaml
-rw-rw-r-- 1 ec2-user ec2-user 168 Jul 28 08:05 04-imagepullpolicy.yaml
-rw-rw-r-- 1 ec2-user ec2-user 307 Jul 28 08:14 05-resource.yaml
-rw-rw-r-- 1 ec2-user ec2-user 453 Jul 28 08:19 06-limitrange.yaml
-rw-rw-r-- 1 ec2-user ec2-user 237 Jul 28 08:32 07-probe.yaml
-rw-rw-r-- 1 ec2-user ec2-user 194 Jul 28 08:40 08-labels.yaml
-rw-rw-r-- 1 ec2-user ec2-user 214 Jul 28 08:42 09-annotations.yaml
-rw-rw-r-- 1 ec2-user ec2-user 283 Jul 28 08:46 10-env.yaml
-rw-rw-r-- 1 ec2-user ec2-user 526 Jul 28 09:04 11-envconfig.yaml
-rw-rw-r-- 1 ec2-user ec2-user 217 Jul 28 09:04 12-envcompleteconfig.yaml
-rw-rw-r-- 1 ec2-user ec2-user 379 Jul 28 09:12 13-envsecret.yaml
-rw-rw-r-- 1 ec2-user ec2-user 526 Jul 28 09:30 14-svcclusterip.yaml
-rw-rw-r-- 1 ec2-user ec2-user 410 Jul 28 09:35 15-svcnodeport.yaml
-rw-rw-r-- 1 ec2-user ec2-user 430 Jul 28 09:41 16-svcloadbalancer.yaml
drwxrwxr-x 2 ec2-user ec2-user  31 Jul 28 09:00 configmaps
drwxrwxr-x 2 ec2-user ec2-user  28 Jul 28 09:08 secrets

```

### FOR REAL TIME PORJECT REFER THE BELOW LINKS DIARAMS

```
https://github.com/sivadevopsdaws74s/roboshop-documentation
https://github.com/techworldwithsiva/k8-roboshop/blob/master/roboshop-k8.jpg
https://github.com/devopsdevopsaws/kubernetes/tree/master/realtimeproject

```
---------------------------------------------------------------------------------------
## REPLICASET

Replicaset can be used for high availblity and it is similar to ASG in AWS , understanding replica set is like it will continous create replica and monitor the pods if any pods get deleted it ll recreated the pods based on the replicas mentioned. 

*syntax is below*

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: nginx
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 4
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx:1.24.0

```

*Disadvantage of the replicaset*
--------------------------------

The major disadvantage of the replicaset is if the image got upgraded or any changes in the image it will not applicabel to the replcia set 

the disadvantage can be overcomed by Deployments in kubernetes.

-------------------------------------------------------------------------------

--------------------------------------------------------------------------------
## Deployment

Advantage of deployment over replcia set is it will upgrade the images as well and it will take care of the changes with zero downtime

*syntax*
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  # modify replicas according to your case
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25.0

```

It uses rolling mechanism technique and it achives zerodowntime.

suppose u have 5 pods which are running rolling update will launch new pod as 6th pod once the new pod is up then the old pod will get deleted in that case there wont be any chances of downtime to the end-user/customer

*the below parameters can be used in the deployment to make use of rollign updates*
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1 ---> suppose 5 pods are available this much loss we can take means 1pod
      maxSurge: 2 --->  number of new pods created while rolling update
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

suppose in the above example if u upgrade from the image from 1.14 to 1.15 and so on

with the help of the below commands u can roll out to previous version very easily 

```
kubectl rollout status deployment/nginx-deployment

kubectl rollout history deployment/nginx-deployment

kubectl rollout history deployment/nginx-deployment --revision=2

kubectl rollout undo deployment/nginx-deployment --> for n-1

kubectl rollout undo deployment/nginx-deployment --to-revision=2 --> to specific version
```

------------------------------------------------------------------------------------------------

## Kubernetes Advanced scheduling concepts
```
*Node Name*
*Node selector*
*Node Affinity*
*Pod Affinity*
*taint*
*tolarence*
```
Below images
------------
