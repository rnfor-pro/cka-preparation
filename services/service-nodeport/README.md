# Service NodePort
[Type: NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#:~:text=same%20IP%20address.-,type%3A%20NodePort,-If%20you%20set)

The Kubernetes service is an object, just like pods replicaset or deployments that we worked with before.
One of its use cases is to listen to a port on the node and forward requests on that port to a port on the pod running the web application.
This type of service is known as a node port service because the service listens to a port on the node and forward requests to the pod.

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
