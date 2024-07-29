Important commands
********************

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
```
### COVER below concepts of kubernetes 
*before going to realtime project*
*Refer Hand book frst*
```
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

**Disadvantage of the replicaset**

The major disadvantage of the replicaset is if the image got upgraded or any changes in the image it will not applicabel to the replcia set 

the disadvantage can be overcomed by Deployments in kubernetes.

