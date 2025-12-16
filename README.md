# RabbitMQ
A end to end Guide for RabbitMQ


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

```
apiVersion: rabbitmq.com/v1beta1
kind: RabbitmqCluster
metadata:
	name: rabbitmq
```

Command:

```
kubectl apply -f the_above_file.yaml
```








