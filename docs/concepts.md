- [Pods](#Pods)
- [Labels](#Labels)
- [Annotations](#Annotations)
- [Replica Sets](#ReplicaSets)
- [Deployments](#Deployments)
- [Services](#Services)
- [Volumes](#Volumes)
- [Config Maps](#ConfigMaps)
- [Secrets](#Secrets)
- [Daemons](#Daemons)
- [Jobs](#Jobs)
- [Namespaces](#Namespaces)

<a name="Pods"></a> 
### Pods

A group of one or more containers is called a pod. Containers in a pod are deployed together, and are started, stopped, and replicated as a group. 

List pods 

```
kubectl get pods -o wide
```

Describe the pod:

```
kubectl describe pod <pod_name>
```

Delete pod:

```
kubectl delete pod nginx
```

Phases of pod :

- **Pending**: the API Server has created a pod resource and stored it in etcd, but the pod has not been scheduled yet, nor have container images been pulled from the registry.
- **Running**: the pod has been scheduled to a node and all containers have been created by the kubelet.
- **Succeeded**: all containers in the pod have terminated successfully and will not be restarted.
- **Failed**: all containers in the pod have terminated and, at least one container has terminated in failure.
- **Unknown**: The API Server was unable to query the state of the pod, typically due to an error in communicating with the kubelet.


<a name="Labels"></a> 
### Labels

Labels are a system to organize objects into groups.

Labels are key-value pairs that are attached to each object. 

Example attach label: 

```
 kubectl label pod nginx type=webserver

 kubectl label node kuben01 rack=rack01
 ```

 <a name="Annotations"></a> 
### Annotations

Similar to labels, but they can’t be used to group objects.

Objects cannot be selected through annotation selectors like label selectors.

Annotations can hold much larger pieces of information than labels

<a name="ReplicaSets"></a> 
### Replica Sets

Ensures that a specified number of pod replicas are running at any one time.

Makes sure that a pod or homogeneous set of pods are always up and available. 

Configuration consists of:

- The number of replicas desired
- The pod definition
- The selector to bind the managed pod

List and describe replica set:

```
kubectl get rs
kubectl describe rs <rs_name>
```

Scale (up/down) replica:

```
kubectl scale rs <rs_name> --replicas=3
```

Delete replica set:

```
kubectl delete rs/nginx
```

<a name="Deployments"></a> 
### Deployments

A Deployment provides declarative updates for pods and replicas.

Two ways of updating an application:

- add all the new pods and then deleting all the old ones at once
- add new pods at time and then removing old ones one by one

The Rolling Update strategy:

Removes some old pods, while adding new ones at the same time, keeping the application available during the process and ensuring there is no lack of handle the user’s requests

`maxSurge` : number of pod instances exist above the desired replica count configured on the deployment

`maxUnavailable`: number of pods can be unavailable below to the desired replica count during the update.

<a name="Services"></a> 
### Services

Defines a logical set of pods and a policy by which to access them

Get service:

```
kubectl get service
```

Describe the service:

```console
# kubectl describe svc nginx                                                                                    

Name:              nginx
Namespace:         default
Labels:            run=nginx
Annotations:       <none>
Selector:          run=nginx
Type:              ClusterIP
IP:                10.59.249.91
Port:              <unset>  8000/TCP
TargetPort:        80/TCP
Endpoints:         10.56.0.13:80,10.56.0.14:80,10.56.1.15:80 + 2 more...
Session Affinity:  None
Events:            <none>
```

`Selector: run=nginx` - it tell that all pods with the label run=nginx are associated to this service.

<a name="Volumes"></a> 
### Volumes

As docker, uses the concept of data volume

2 particular volume types:

#### emptyDir

This type of volume is created when the pod is assigned to a node, and it exists as long as that pod is running on that node. It is initially empty. When the pod is removed from the node for any reason, the data in the emptyDir volume is deleted too.

#### hostPath

Volume is mount from an existing directory on the file system of the node hosting the pod. 

Data inside the host directory are safe to container crashes and restarts as well as to pod deletion

However, if the pod is moved from a node to another one, data on the initial node are no more accessible from the new instance of the pod.


<a name="ConfigMaps"></a> 
### Config Maps

Kubernetes allows separating configuration options into a separate object called ConfigMap

- Mount Config Map as volume: config Map is mounted as a volume into the pod
- Pass Config Map by environment variables

<a name="Secrets"></a> 
### Secrets

Secrets are kept safe by distributing sensitive information (such as credentials, tokens and private encryption keys) only to the nodes that run the pods that need access to the secrets.

On the nodes, secrets are always stored in memory and never written to the disk.

On the master node, secrets are stored in encrypted form into the etcd database.

<a name="Daemons"></a> 
### Daemons

A Daemon Set is a controller type ensuring each node in the cluster runs a pod.

New node is added to the cluster, a new pod is added to the node. 

The node is removed from the cluster, the pod running on it is removed and not scheduled on another node. 

Deleting a Daemon Set will clean up all the pods it created.

Some typical uses of Daemon Set:

- running a cluster storage daemon, such as `glusterd`, `ceph`, on each node.
- running a logs collection daemon on every node, such as `fluentd` or `logstash`.
- running a node monitoring daemon on every node, such as Prometheus Node Exporter, `collectd`, Dynatrace OneAgent, Datadog agent, New Relic agent, Ganglia gmond or Instana agent.

<a name="Jobs"></a> 
### Jobs

 Job is an abstraction for create batch processes. A job creates one or more pods and ensures that a given number of them successfully complete.

<a name="Namespaces"></a> 
### Namespaces

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces. 

Two initial namespaces:

- default: the default namespace for objects with no other namespace
- kube-system the namespace for objects created by the kubernetes system