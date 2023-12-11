# Voting app on kubernetes pods

Our goal is to deploy these applications as containers on a Kubernetes cluster , and then enable connectivity between the containers so that the applications can access each other and the databases and then enable external access for the external facing applications which are the voting and the result app so that the users can access the web browser.

# Deployment phase

So how do we go about this?
We know that we cannot deploy containers directly on Kubernetes.
We learned that the smallest object that we can create on a Kubernetes cluster is a pod.
So we must first deploy these applications as a pod on our Kubernetes cluster.
Or we could deploy them as replica assets or deployments as we have seen through throughout this course.
But at first, for the sake of simplicity, we will stick to pods in this lecture, and later we will
see how to easily convert that to a deployments.

# Enabling connectivity between services

So once the pods are deployed, the next step is to enable connectivity between the services.
So it's important to know what the connectivity requirements are.
We must be very clear about what application requires access to what services. We know that the redis database is accessed by the voting app and the worker app.
The voting app saves the vote to the Redis database, and the worker app reads the vote from the Redis database

We know that the PostgreSQL database is accessed by the Worker app to update it with the total count of votes, and it's also accessed by the result app to read the total count of votes to be displayed in the resulting web page in the browser.

We know that the voting app is accessed by the external users, the voters, and the result app is also accessed by the external users to view the results.
Most of the components are being accessed by other components except for the worker app.

While the voting app has a Python web server that listens on Port 80 and the result app also has a Node.js based server that listens on Port 80, and the Redis database has a service that listens on Port 6379 and the PostgreSQL database has a service that listens on Port 5432.
The worker app has no service because it's just a worker and it's not accessed by any other service or external users.

# Enabling access between the different components

How do you make one component accessible by another?
Say, for example, how do you make the Redis database accessible by the voting app?
Should the voting app use the IP address of the redis? Perhaps no, because that can change.
The IP of the pod can change if the port restarts. And you may also run into issues when you try to scale your applications in the future, the right way
to do it is to use a service.
Now we learned that a service can be used to expose an application to other applications or users for external access.
So we will create a service for the redis pod so that it can be accessed by the voting app and the worker app.
And we will call it a redis service and it will be accessible anywhere within the cluster by the name of the service redis.
So why is that name important?
The source code within the voting app and the worker app are hardcoded to point to a Redis database
running on a host by the name Redis.
So it's important to name your service as redis.

So that these applications can connect to the Redis database.
And this is not a best practice to hard code stuff like this within the source code of an application.
Instead, you should be using environment variables or something, but for the sake of simplicity, we will just follow this application as it is developed.

Right now, these services are not to be accessed outside the cluster, so they should just be of type
cluster IP.
So we will follow the the same approach of creating a service for the PostgreSQL pod so that the PostgreSQL
DB can be accessed by the worker and the result app.
So what should we name the PostgreSQL Service?
If you look at the source code of the result app and the worker app, you will see that they are looking for a database at the address.
DB So the service that we create for PostgreSQL should be named DB.

Also note that while connecting to the database, the worker and the result apps passing a username and password to connect to the database, both of which are set to Postgres.
So when we deploy the Postgres DB pod, we must make sure that we set the these credentials for it as the initial set of credentials to while creating the database.
Now the next task is to enable external access.
So for this we saw that we could use a service with a type set to node port.
So we create services for voting app and the result app and set their type to node port. Now we could decide on what port we are going to make them available on and it would be a high port
with a port number greater than 30000. So we'll do that when we create the service.
So we are done and we have the high level steps ready.
So to summarize, we will be deploying five ports in total and we have four services, one for Redis, another for PostgreSQL, both of which are internal services.
So they are of type cluster IP and we then have external facing services for voting app and the result app.
However, we have no service for the worker pod and this is because it is not running any service that must be accessed by another application or external users.
So it is just a worker process that reads from one database and updates another.

So it's not going to require a service.
Now I say that again as that's a common question that I get when we talk about services.
Why does this worker not require a service?

Right.
So a service is only required if the application has some kind of process or database service or web service that needs to be exposed, that needs to be accessed by others in this case, that's that's
not true for the worker app. Now, before we we get started with the deployment node that we will be using the following Docker images
for these applications.
So these images are built from a fork of the original developed at the Docker Samples repository. 

