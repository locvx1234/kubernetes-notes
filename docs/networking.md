# Cluster networking 

### Pod networking 

When a pod is deployed, it gets an IP address from the cluster IP address range defined in the inital setup.

Both pods run on the same host node, as we see from their IP address. We can still access both pods from any other node in the cluster

We do not need to expose container port on host to access application as it is required in standard docker networking model.

### Host Networking

Define pods to use the same host IP address with ` hostNetwork: true`.

However, with the `hostNetwork: true` we cannot start more than one pod listening on the same host port.

In general, pods with host network are only used for system or daemon applications that do not need to be scaled.

### Exposing services

Services are used not only to provides access to other pods inside the same cluster but also to clients outside the cluster. 

Service types:

- **ClusterIP** (default): exposes the service only on a cluster internal IP making the service only reachable from within the cluster.
- **NodePort**: exposes the service on each node public IP on a static port as defined in the NodePort option. It will be possible to access the service, from outside the cluster.
- **LoadBalancer**: exposes the service by creating an external load balancer. It works only on some public cloud providers. To make this working, remember to set the option --cloud-provider in the kube controller manager startup file.

### Service discovery

To enable service discovery, we need to configure an embedded DNS service to resolve all DNS queries from pods trying to access services.

If we want service discovery but would rather have the DNS service return the IP addresses of the pods rather than the service IP, set `clusterIP: none`


