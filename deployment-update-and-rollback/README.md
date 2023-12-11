# Rolling updates and Rollback
[Performing a Rolling Update](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/)
```yaml
kubectl create -f deployment.yaml
kubectl get deploy
kubectl describe deployment.apps/myapp-deployment
```
Observe the output

```yaml
kubectl rollout history deployment.apps/myapp-deployment
```
You can see that the myapps deployment has one revision, which is the one that we just did that was creating the deployment itself.
And there is also a column called Change Costs, which you'll notice that has there is no change cost specified.
And that is because we did not specifically ask it to record the cause of change while we created the
deployment.

Let's go back and fix that.

```yaml
kubectl delete deployment myapp-deployment
kubectl get pods 
```
Wait for the pods to go away.

```yaml
kubectl create -f deployment.yaml --record
```
The record option instructs Kubernetes to record the cause of change.

```yaml
kubectl rollout status deployment.apps/myapp-deployment
```
Run the rollout history again to get the cause of the change

```yaml
kubectl rollout history deployment.apps/myapp-deployment
```
This time you'll see that against the revision there is a change course which is mentioned, which
is essentially the same command that we ran to create the deployment.

Make a change to the deployment.
describe the deployment and take note of the image version used.

```yaml
kubectl set image deployment myapp-deployment nginx=nginx:1.18-pearl --record
```

```yaml
kubectl rollout status deployment.apps/myapp-deployment
```

```yaml
kubectl rollout history deployment.apps/myapp-deployment
```
We can revert back to the previous version by using the undo command.

```yaml
kubectl rollout undo deployment.apps/myapp-deployment
```
describe the deployment and take note of the image version.

run the below command again and observe the output

```yaml
kubectl rollout history deployment.apps/myapp-deployment
```
Now you'll see that we still have three revisions in total.
There is a new revision which is now created, which is number four, but the revision number two is gone.
Now, that's because the fourth revision, which is the new revision, is essentially the just the same as the second revision.

Edit the deployment to use an image that does not exist.
Run the below command

```yaml
kubectl rollout status deployment.apps/myapp-deployment
```
It's going to stay there as it's trying to update to an image that does not exist.
And it's not able to pull that image from Docker Hub so the containers would fail.

Run the 

```yaml
kubectl get deploy myapp-deployment
```
Observe the output.
And here you'll notice that the number of pods which are running is actually five.
So here the deployment has terminated one of the existing pods on version 1.18, and it has tried to
create three new pods with the new version of the image.
The one that does not exist.

So even though we have specified a wrong image and there are three new parts which are trying to load
the new image, the application is not impacted and the end users will still be able to access the application
on the old five running pod's.

And this is because since we set the strategy rolling up grade Kubernetes will only downgrade or destroy
the older parts once we have sufficient newer parts available.

```yaml
kubectl rollout history deployment.apps/myapp-deployment
```
So now let's take a look at the revision history, and you will see that a new revision, which is revision
number five, is recorded as well.

So now as a final step, let us do an undo deployment operation so that it goes from revision number
five back to revision number of four and fix the name of the image back to engine X 1.18.

```yaml
kubectl rollout undo deployment.apps/myapp-deployment
```
```yaml
kubectl rollout status deployment.apps/myapp-deployment
```
```yaml
kubectl get deployment
```
Describe your deployment and look at the version.