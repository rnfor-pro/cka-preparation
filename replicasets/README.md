# Controllers
Controllers are the brain behind Kubernetes. They are the processes that monitor Kubernetes objects and respond accordingly.

## Replication Controller
The replication controller ensures that the specified number of pods are running at all times, even if it's just one or 100. Another reason we need replication controller is to create multiple ports to share the load across them. the replication controller spans across multiple nodes in the cluster. Replication controller is the older technology that is being replaced by  [replica set](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)


## Replicaset
[ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

Replica set is the new recommended way to set up replication.

Why do we label our pods and objects in Kubernetes?

It can be used to monitor existing parts if you have them already created.
In case they were not created, the replica set will create them for you.
The role of the replica set is to monitor the pods and if any of them were to fail deploy new ones.
The replica set is in fact a process that monitors the pods.

How does the replica set know what pods to monitor?
It makes use of the labels and selectors.


