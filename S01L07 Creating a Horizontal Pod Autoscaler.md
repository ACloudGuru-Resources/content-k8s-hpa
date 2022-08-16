## S01L07 Creating a Horizontal Pod Autoscaler
Begin by creating a Deployment which we will autoscale.

```
vi scaling-deploy.yml
```

This deployment uses the `resource-consumer` image, which we can use later to simulate load and test our autoscaling functionality. Note that resource requests are included in the container configuration. HPA uses these as a baseline to determine expected resource usage.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: scaling-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: scaling-deploy
  template:
    metadata:
      labels:
        app: scaling-deploy
    spec:
      containers:
      - name: resource-consumer
        image: gcr.io/kubernetes-e2e-test-images/resource-consumer:1.5
        resources:
          requests:
            cpu: 100m
```

```
kubectl apply -f scaling-deploy.yml
```

View the deployment's Pods.

```
kubectl get pods
```

Create a HorizontalPodAutoscaler.

```
kubectl autoscale deployment scaling-deploy --cpu-percent=50 --min=1 --max=10
```

Check the status of your HPA.

```
kubectl get hpa
```

You may see `<unknown>` for the target. This may mean that metrics server just needs a few more moments to collect data on the new Pods.

Currently, there is no real resource load. If you wait five minutes or so, you may see the HPA scale the deployment down to the minimum replica count of `1`.
