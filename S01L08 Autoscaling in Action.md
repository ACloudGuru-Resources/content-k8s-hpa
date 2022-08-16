## S01L08 Autoscaling in Action
Now we are ready to test our autoscaling.

Get a list of Pods with their cluster IP addresses.

```
kubectl get pods -o wide
```

If the HPA has scaled down, there should be only one replica. Note the IP address of the Pod.

Since we are using the `resource-consumer` image, we can use the Pod's IP address to make an http request to the Pod and instruct it to generate some CPU load. Supply your actual Pod IP address in place of `<Pod IP address>` for this command.

This command will increase the CPU usage of the container to `250` millicores for `2` minutes.
```
curl <Pod IP address>:8080/ConsumeCPU -d "millicores=250&durationSec=120"
```

Check the CPU usage metrics for the Pod. Re-run this command a few times, waiting until the CPU usage increases. You may see additional Pods appear. This is due to the HPA scaling up.

```
kubectl top pod
```

Check the status of the HPA.

```
kubectl get hpa
```

After 2 minutes, the CPU usage of the Pod should go back down. About five minutes after that occurs, the HPA should scale down in response.

If you wish, you can check the resource usage metrics and the status of the HPA to watch this happen.

```
kubectl top pod

kubectl get hpa
```
