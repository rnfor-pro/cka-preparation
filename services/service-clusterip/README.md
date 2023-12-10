# Service Clusterip
[Type: Clusterip](https://kubernetes.io/docs/concepts/services-networking/service/#:~:text=NodePort%20allocation.-,type%3A%20ClusterIP,-This%20default%20Service)

A full stack web application typically has different kinds of parts hosting different parts of an application. You may have a number of parts running a frontend web server, another set of parts running a backend server, a set of parts running a key value store like Redis and another set of parts maybe running a persistent database like my SQL.
The Web frontend server needs to communicate to the backend servers and the backend servers need to communicate to the database as well as the ready services, etc..
So what is the right way to establish connectivity between these services or tiers of my application?
The pods all have an IP address assigned to them, But these IPS, as we know, are not static.
These pods can go down any time and new pods are created all the time. And so you cannot rely on these IP addresses for internal communication between the application.

Also, what if the first front end pod needs to connect to a backend service? Which of the three would it go to and who makes that decision?
A Kubernetes service can help us group the pods together and provide a single interface to access the pods in a group.
For example, a service created for the back end pods will help group all the back end pods together and provide a single interface for other parts to access this service.
The requests are forwarded to one of the pods under the service randomly.
Similarly, create additional services for Redis and allow the back end pods to access the ready systems through the service.
This enables us to easily and effectively deploy a microservices based application on Kubernetes cluster.
Each layer can now scale or move as required without impacting communication between the various services.
Each service gets an IP and name assigned to it inside the cluster, and that is the name that should be used by other pods to access the service.
This type of service is known as cluster IP.