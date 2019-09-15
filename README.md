### Init minikube

`$ sudo minikube start --cpus=2 --memory=2040 --vm-driver=none`

### Create a namespace for RabbitMQ test:

`$ sudo kubectl create namespace test-rabbitmq`

### Deploy

```
$ sudo kubectl create -f ./rabbitmq_rbac.yaml
$ sudo kubectl create -f ./rabbitmq_statefulsets.yaml
```

# Check the cluster status:
Wait few seconds....then run

```
$ FIRST_POD=$(sudo kubectl get pods --namespace test-rabbitmq -l 'app=rabbitmq' -o jsonpath='{.items[0].metadata.name }')
$ sudo kubectl exec --namespace=test-rabbitmq $FIRST_POD rabbitmqctl cluster_statussudo kubectl exec --namespace=test-rabbitmq $FIRST_POD rabbitmqctl cluster_status
```

The output should look something like this:

```
Cluster status of node 'rabbit@172.17.0.2'
[{nodes,[{disc,['rabbit@172.17.0.2','rabbit@172.17.0.4',
                'rabbit@172.17.0.5']}]},
 {running_nodes,['rabbit@172.17.0.5','rabbit@172.17.0.4','rabbit@172.17.0.2']},
 {cluster_name,<<"rabbit@rabbitmq-0.rabbitmq.test-rabbitmq.svc.cluster.local">>},
 {partitions,[]},
 {alarms,[{'rabbit@172.17.0.5',[]},
          {'rabbit@172.17.0.4',[]},
          {'rabbit@172.17.0.2',[]}]}]
```

### Get your minikube ip:

`$ sudo minikube ip`

## Ports:

http://<<minikube_ip>>:31672: HTTP API and management UI
amqp://guest:guest@<<minikube_ip>>:30672: AMQP 0-9-1 and AMQP 1.0

### To delete 

`$ sudo minikube delete`