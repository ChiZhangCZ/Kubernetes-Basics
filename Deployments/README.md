# Deployments

Using a Kubernetes deployment to create, update and rollback a simple hello world webapp hosted on 10 pods

# Creating Deployment

Similar to a replication controller, we can use a deployment to create pods. In this example our deployment will create 10 pods running a simple hello world webapp

- Create the deployment:
```
kubectl create -f deployment.yaml
```

We can expose our pods using the same service we created in the pods and services example, with this service created we can navigate to:
```
http://<cluster IP>:30001
```

We should see a webpage displaying the text "Hello Kubernetes!"

# Updates and Rollbacks

Kubernetes deployments allow for rolling updates and simple rollbacks for our pods. We do this by editing our deployment manifest and pushing it to the Kubernetes API server.

Deployments wrap around replica sets(similar to replication controllers) to manage them. When an update is pushed Kubernetes wll shutdown the current replica set and create a new one.

The old replica set is still present, this means we can rollback updates in a similar way to pushing them. We simply shutdown the new replica set and start the old one. In this example we will update and rollback the image the pods are using

- Update the deployment by editing image value of the deployment.yaml file: 
```
...
image: gcr.io/google-samples/node-hello:1.0
...
```
- Apply the update:
```
kubectl apply -f deployment.yaml --record
```

We can check the progress of your update with:
```
kubectl get deploy hello-deploy
```

Once the values of the "DESIRED","CURRENT" and "AVAILABLE" pods are all equal to 10 the update is complete.

Navigate to:
```
http://<cluster IP>:30001
```

We should now see "Hello, world!" displayed along with the version and hostname.

- Rollback the update:
```
kubectl rollout undo deployment hello-deploy --to-revision=1
```

The "--to-revision=1" label will tell Kubernetes to revert back to the inital deployment.

We can check the rollback progress in the same way we checked our update progress. Once it is complete the webpage should once again display "Hello Kubernetes!"
