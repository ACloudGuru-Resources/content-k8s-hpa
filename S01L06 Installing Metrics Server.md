## S01L06 Installing Metrics Server

Install metrics server in the cluster.

```
kubectl apply -f https://raw.githubusercontent.com/ACloudGuru-Resources/content-cka-resources/master/metrics-server-components.yaml
```

Check the status of the metrics server Pod.

```
kubectl get pods -n kube-system
```

You should see a `metrics-server` Pod. It may take a few minutes for the metrics server container to become ready.

Once the metrics server container is ready, test the metrics server by retrieving some metric data. This command displays resource usage for nodes in the cluster. Note that it may take a moment for metrics server to collect the initial set of data. If you get no data at first, wait a few moments and try again.

```
kubectl top node
```
