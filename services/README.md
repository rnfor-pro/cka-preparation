# Services
[Services](https://kubernetes.io/docs/concepts/services-networking/service/)

There are 3 main kinds of services available which we will discuss.
The first one is the `nodeport service` where the service makes an internal port accessible on a port on the node.

The second is `cluster IP`.
In this case, the service creates a virtual IP inside the cluster to enable communication between different services, such as a set of frontend servers to a set of backend servers.

The third type is a `load balancer`, where it provisions a load balancer for our application in supported cloud providers.
A good example of that would be to distribute load across the different web servers in your front end tier.

Let's take a closer look at the `node port service`. If you look at it, there are three ports involved.
The port on the pod where the actual web server is running is 80.
And it is referred to as the target port, because that is where the service forwards the request to.
The second port is the port on the service itself.
It is simply referred to as the port.
Remember, these terms are from the viewpoint of the service.
The service is in fact like a virtual server inside the node, inside the cluster, it has its own IP address, and that IP address is called the cluster IP of the service.
And finally, we have the port on the node itself, which we use to access the web server externally, and that is known as the node port.
It could be set to 30008.
That is because node ports can only be in a valid range, which by default is from 30000 to 32767.