### 1. **What is a Service in Kubernetes, and why is it needed?**
A **Service** in Kubernetes is an abstraction that defines a logical set of Pods and a policy to access them. Since Pods are ephemeral and can change over time (due to scaling, rescheduling, or failures), a Service provides a stable endpoint for accessing them. It enables communication between different parts of an application (e.g., between frontend and backend components) or between external clients and the app running in Kubernetes.

### 2. **What are the different types of Services in Kubernetes?**

- **ClusterIP** (default): Exposes the service on an internal IP address in the cluster. This means the service is only accessible from within the cluster. Use case: Internal communication between microservices.
  
- **NodePort**: Exposes the service on a static port on each node in the cluster. It allows external traffic to access the service by connecting to the node's IP and the NodePort. Use case: When you need to expose services externally without a LoadBalancer.

- **LoadBalancer**: Creates an external load balancer (if your cloud provider supports it) and assigns a public IP to the service. Use case: Exposing services to the internet via a cloud provider.

- **ExternalName**: Maps a service to an external DNS name by returning a CNAME record. It’s a way to access external services from within the Kubernetes cluster. Use case: When you need to reference external services using DNS.

### 3. **What are LoadBalancer and ExternalName services in Kubernetes, and what are their use cases?**

- **LoadBalancer**: This service provisions a load balancer from the cloud provider, allowing external access to the service. It’s typically used when you need to expose a service to the internet.

- **ExternalName**: This service does not create any proxy or forwarding rules. Instead, it simply maps the service to an external DNS name. It's useful when you want to access external services (like a database or external API) via DNS within the cluster.

### 4. **How does a Kubernetes Service use selectors to identify the Pods it routes traffic to? Can a Service be created without a selector?**

A Kubernetes service uses **selectors** to map the service to a set of Pods. The service routes traffic to the Pods that match the specified label selector, which is a key-value pair (e.g., `app: my-app`).

Yes, a service can be created without a selector. In such cases, it will not route traffic to any pods but can be used with endpoints (manually defined) or used for external name services.

### 5. **Explain how a NodePort service works in Kubernetes. What are its advantages and limitations?**

A **NodePort** service exposes the service on a static port on all nodes in the cluster. External traffic can access the service by connecting to the node’s IP and the NodePort.

- **Advantages**: 
  - Easy to expose services to the outside world without requiring a load balancer.
  - Can work in on-premise environments without cloud provider support.

- **Limitations**: 
  - Exposes the service on a high port (30000-32767), which may not be ideal for production use.
  - Scalability is limited since it doesn't provide load balancing across nodes automatically.

### 6. **What is a Headless Service in Kubernetes, and when would you use it? How does it differ from a standard ClusterIP service?**

A **Headless Service** is a service without a cluster IP, meaning Kubernetes doesn’t provide a load-balancing proxy. Instead, the DNS resolves directly to the Pod’s IP addresses. It is used when you need to handle the service discovery or load balancing yourself, such as with stateful applications (e.g., databases, Kafka).

- **Difference from ClusterIP**: Unlike ClusterIP, which provides a virtual IP and load balances across the pods, a headless service returns the individual Pod IPs.

---
### 7. **Create a ClusterIP Service YAML for `my-app`**

![](c46ac82322ba529d30967067d3f76896.png)
### 8. **Set up a NodePort Service for `backend` on port 30001**
backend.yaml
![](f701aea83f51c57c21d912ed135c83cb.png)
`kubectl apply -f backend-pod.yaml`
`kubectl apply -f backend.yaml`
						`minikube service backend-nodeport --url`
and the service is exposed on 
http://192.168.39.62:30001
![](7b0f5553790a8dba229533f6d93ad6e3.png)
Success!
### 9. **Test Service with Selectors: ClusterIP Service for `old-web`**

![](4b9295a83a4dd7d97306d11edbb1c9ee.png)
`kubectl get endpoints web-service`
![](90a2c3f2328eb0ee59ad766ea867c5ad.png)
**Test Traffic Routing**:
Create a temporary Pod with `curl` installed to test the service:
![](262d748ad24006193a07c5f05985361b.png)
`kubectl apply -f temp-pod.yaml`
test with curl
`kubectl exec -it curl-pod -- curl web-service:80`
![](848679dcd38aa54a018b12ebbaa11943.png)
I created a ClusterIP service named `web-service` with the selector `app: old-web`. You deployed two sets of Pods with different labels and verified that only Pods with the label `app: old-web` receive traffic by using `curl` from a temporary Pod. This confirms that the service is correctly routing traffic based on the selector.
### 10. **Change Service Selector**

First I created a new pod like the old one but with new-web label

Then I checked the endpoints for the service again:
`kubectl get endpoints web-service`

Now, the IP address of the `new-web-pod` should appear. The service will no longer route traffic to the `old-web-pod`.

![](f12d02b3bff7d77de74daf821fddb0ed.png)

### 11. **Configure a Service for Multiple Ports**

1. **Create a Kubernetes Deployment for Your App**
		the code in multi-port-deployment.yaml

2. **Create a ConfigMap to Configure Nginx for HTTP/HTTPS**
		the code in configMap.yaml

3. **Create a Kubernetes Service to Expose Multiple Ports**
		the code in multi-port-service.yaml

4. Create SSL Certificates

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 \   -keyout nginx.key \   -out nginx.crt \   -subj "/CN=localhost"`
After generating the SSL certificates, mount into the Nginx container using a Kubernetes secret or mounted from a hostPath.

5. Create a Kubernetes Secret to Store SSL Certificates
		the code in nginx-ssl.yaml

then apply the files and 

Since `ClusterIP` type services are only accessible within the cluster, you can use `kubectl port-forward` to access the service from your local machine:
```sh
kubectl port-forward svc/multi-port-service 8080:80
kubectl port-forward svc/multi-port-service 8443:443
```
You can then access the service locally at `http://localhost:8080` for HTTP and `https://localhost:8443` for HTTPS.

Test HTTP and HTTPS
![[Screenshot_20240913_142639.png]]
