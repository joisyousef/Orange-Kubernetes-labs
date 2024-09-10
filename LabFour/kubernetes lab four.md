Here is the content formatted in **Markdown** for GitHub:

```markdown
# Kubernetes Namespace: Q&A

## 1. What is a namespace in Kubernetes, and why is it used?
A **namespace** in Kubernetes is a way to divide cluster resources between multiple users or teams. It allows you to create isolated environments within the same cluster, preventing naming conflicts and enabling better resource management. It is often used to organize and manage workloads for different projects, teams, or stages of development (like `dev`, `staging`, and `production`).

---

## 2. How do you create a new namespace in Kubernetes using the kubectl command?
You can create a new namespace with the following command:
```bash
kubectl create namespace <namespace-name>
```
Example:
```bash
kubectl create namespace dev-environment
```

---
## 3. How can you list all namespaces in a Kubernetes cluster?
To list all namespaces in the cluster, use:
```bash
kubectl get namespaces
```

---
## 4. What is the default namespace in Kubernetes? What happens if you do not specify a namespace when deploying a resource?

The default namespace is **`default`**. If you do not specify a namespace when deploying a resource, Kubernetes automatically deploys the resource to the **default** namespace.

---
## 5. How do you delete a namespace in Kubernetes? What happens to the resources within it?

To delete a namespace, run:
```bash
kubectl delete namespace <namespace-name>
```
When you delete a namespace, all resources (pods, services, deployments, etc.) within that namespace are deleted along with it.

---
## 6. How can you switch between namespaces while using the kubectl command?

You can switch between namespaces by specifying the `-n` or `--namespace` flag:
```bash
kubectl get pods -n <namespace-name>
```
Example:
```bash
kubectl get pods -n dev-environment
```

---
## 7. How do you create a Kubernetes deployment in a specific namespace?

You can create a deployment in a specific namespace by adding the `--namespace` flag to your `kubectl` command:
```bash
kubectl create deployment <deployment-name> --image=<image-name> --namespace=<namespace-name>
```
Example:
```bash
kubectl create deployment my-deployment --image=nginx --namespace=dev-environment
```

---
## 8. Can two different namespaces have resources with the same name? Explain your answer.

Yes, two different namespaces can have resources with the same name. Namespaces create isolated environments, so resources in different namespaces do not conflict with each other, even if they have the same name.

---
## 9. How can you check the resource quotas and limits for a specific namespace?

You can check the resource quotas and limits for a namespace using:
```bash
kubectl get resourcequota -n <namespace-name>
```
For detailed information:
```bash
kubectl describe resourcequota <resource-quota-name> -n <namespace-name>
```

---
## 10. How do you configure a kubectl context to always use a specific namespace by default?

You can set a specific namespace in your current context with this command:
```bash
kubectl config set-context --current --namespace=<namespace-name>
```
Example:
```bash
kubectl config set-context --current --namespace=dev-environment
```

---
## 11. Create a YAML file to define a new namespace called dev-environment. Deploy it using kubectl.

Here's the YAML file (`namespace.yaml`):
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-environment
```

Deploy it using:
```bash
kubectl apply -f namespace.yaml
```
![](29e94684126a9ce9d81b165ec1828ef7.png)

---
## 12. Write a command to deploy a pod named test-pod running the nginx image into a namespace called testing.

```bash
kubectl run test-pod --image=nginx --namespace=testing
```

---
## 13. List all the pods running in a namespace called production.
```bash
	kubectl get pods -n production
```
