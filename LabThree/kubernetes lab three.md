### 1. Create a Deployment called `my-first-deployment` of image `nginx:alpine` in the default namespace. Check to make sure the deployment is healthy.

![](attachment/1e5bb74591805b45bfb8263f6e98b2aa.png)
Created a yaml file called my-first-deployment

**READY** column shows `1/1` for the pod to verify that it's healthy.

### 3. Scale `my-first-deployment` down to run 2 replicas.
```
kubectl scale deployment my-first-deployment --replicas=2
```

```
kubectl get deployment my-first-deployment
```

![](attachment/4d9d88536aa4c1c5a1c12c3d29242cdd.png)

### 4. Change the image `my-first-deployment` runs from `nginx:alpine` to `httpd:alpine`.

```
kubectl set image deployment/my-first-deployment nginx-container=httpd:alpine
kubectl get deployment my-first-deployment
```
![](attachment/d210c00d16dc5e6af279cb8a86119a49.png)
also check the status of the new pods to ensure they are running the `httpd:alpine` image:
![](attachment/47b8ef33436674e83b1ae26dcd2c39fc.png)

### 5. Delete the deployment `my-first-deployment`.

```
kubectl delete deployment my-first-deployment
```

![](attachment/daaf8285d136a2928491a62a6679af29.png)

### 6. Create a deployment using the provided YAML:

![](attachment/483217df646f565662a0e6e7ba2d1496.png)
### 7. How many ReplicaSets exist on the system now?

![](attachment/c7e1cf108f79f4513ccd5dfd680bbdb7.png)
### 8. How many PODs exist on the system now?
get pods:
![](attachment/6dbb361cacb366392e317b5a5afb2e12.png)

### 9. Out of all the existing PODs, how many are ready?
also use get pods:
![](attachment/6dbb361cacb366392e317b5a5afb2e12.png)
### 10. What is the image used to create the pods in the new deployment?
The image used to create the pods is **busybox888**, as specified in the provided YAML under the `containers` section.
```
containers: 
  - name: busybox-container 
    image: busybox888
```

### 11. Why do you think the deployment is not ready?

If the deployment is not ready, some common reasons might include:

- **Image Pull Issues**: The image `busybox888` might not exist or might be misconfigured, preventing Kubernetes from pulling it.
- **Command Error**: The command `echo Hello Kubernetes! && sleep 3600` might not be working as expected inside the container.
- **Pod Scheduling Issues**: There might not be enough resources available on the nodes to schedule the pods.

![](attachment/5493414c1cb7c38f9bfdf129105590a1.png)