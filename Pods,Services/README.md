# Pods and Services

A simple backend running on 5 pods exposed via nodeport using a service.

# Creating Pods

In this example we are using a replication controller to create 5 pods running a simple hello world web app

Create the replication controller with the command:
```
kubectl create -f replication-controller.yaml

```
We can view our replication controller with:
```
kubectl get rc
```

After some time 5 pods should be running, we can check this with:
```
kubectl get pods
```

If we use the kubectl delete command to delete any of these pods, the replication controller will automatically deploy a new pod to fulfill the desired state of 5.

# Exposing Pods


We can expose our pods using Kubernetes services, in this example we will use a NodePort service to expose our pods.

Create the service:
```
kubectl create -f service.yaml
```

The service will use labels to determine which pods to expose, here we have configured the service to expose the pods created by our replication controller. A Nodeport service will expose our pods on a cluster-wide port, in this case 300001.

# Testing

Navigate to:
```
http://<cluster IP>:30001
```

We should see a webpage displaying the text "Hello Kubernetes!"
