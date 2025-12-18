# RabbitMQ
A end to end Guide for RabbitMQ


## Insrallation Process

Let's start by installing the latest version of the Cluster Operator. This can be done directly using kubectl apply:

```
kubectl apply -f "https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml"

```


Output:

```
# namespace/rabbitmq-system created
# customresourcedefinition.apiextensions.k8s.io/rabbitmqclusters.rabbitmq.com created
# serviceaccount/rabbitmq-cluster-operator created
# role.rbac.authorization.k8s.io/rabbitmq-cluster-leader-election-role created
# clusterrole.rbac.authorization.k8s.io/rabbitmq-cluster-operator-role created
# rolebinding.rbac.authorization.k8s.io/rabbitmq-cluster-leader-election-rolebinding created
# clusterrolebinding.rbac.authorization.k8s.io/rabbitmq-cluster-operator-rolebinding created
# deployment.apps/rabbitmq-cluster-operator created
```


Operator has created the following resources for us:

```

kubectl get all -n rabbitmq-system

NAME                                             READY   STATUS    RESTARTS   AGE
pod/rabbitmq-cluster-operator-848b8f777d-2xjpj   1/1     Running   0          18m

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/rabbitmq-cluster-operator   1/1     1            1           18m

NAME                                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/rabbitmq-cluster-operator-848b8f777d   1         1         1       18m


```


A new CRD (Custom Resource Definition) has been created for us and using this we can extend kubernetes API to create our own RabbitMQ cluster as a custom resource in kubernetes.


In the next step we will apply the CR on our cluster using the following file:


But before create a namespace rabbitmq:

```
kubectl cretae ns rabbitmq
```

Then you can proceed applying this file on your cluster

```
apiVersion: rabbitmq.com/v1beta1
kind: RabbitmqCluster
metadata:
	name: rabbitmq
    namespace: rabbitmq
```

Command:

```
kubectl apply -f the_above_file.yaml
```


After Application you can see the resources in the rabbitmq namespace that are created:

```
 kubectl get all -n rabbitmq

NAME                    READY   STATUS    RESTARTS   AGE
pod/rabbitmq-server-0   1/1     Running   0          4m22s

NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                        AGE
service/rabbitmq         ClusterIP   10.110.52.114   <none>        5672/TCP,15672/TCP,15692/TCP   4m23s
service/rabbitmq-nodes   ClusterIP   None            <none>        4369/TCP,25672/TCP             4m23s

NAME                               READY   AGE
statefulset.apps/rabbitmq-server   1/1     4m24s

NAME                                    ALLREPLICASREADY   RECONCILESUCCESS   AGE
rabbitmqcluster.rabbitmq.com/rabbitmq   True               True               4m25s

```


You can view the custom resource as well:

```
 kubectl get rabbitmqclusters -n rabbitmq

NAME       ALLREPLICASREADY   RECONCILESUCCESS   AGE
rabbitmq   True               True               6m34s
```


## How to Find the Connection URL:

To conenct Rabbitmq you will need a URL, it is of the following form:

```
RABBITMQ_URL=amqp://$(USERNAME):$(PASSWORD)@rabbitmq.rabbitmq.svc.cluster.local:5672/
```

To find the username and password we will use the secrets that were generated during our deployment:

```
kubectl get secret rabbitmq-default-user -n rabbitmq -o jsonpath='{.data.username}' | base64 --decode

default_user_Myq5xPXPJEo2N7ksJRO


kubectl get secret rabbitmq-default-user -n rabbitmq -o jsonpath='{.data.password}' | base64 --decode

I_Have_Trust_Issues

```



