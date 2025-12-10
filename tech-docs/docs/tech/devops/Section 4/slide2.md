---
sidebar_position: 2
sidebar_label: K8's Probes
---

# Kubernetes Probes - Liveness and Readiness

1. Imagine you have a web application running in a Kubernetes pod.
2. Over time, the application might encounter issues like deadlocks or
   stuck in while loop (just for fun) causing it to become unresponsive.
3. How does Kubernetes know when your application is healthy or ready to serve traffic?
4. This is where **Liveness Probes** and **Readiness Probes** come
   into play.
5. **Liveness Probe** checks if your application is alive. If it fails,
   Kubernetes will restart the pod.
6. **Readiness Probe** checks if your application is ready to serve traffic.
   If it fails, Kubernetes will stop sending traffic to the pod until it passes again.
7. You can configure these probes using HTTP requests, TCP sockets, or command execution.
8. Here's an example of how to define liveness and readiness probes in a Kubernetes
   deployment YAML:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
     labels:
       app: nginx
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: nginx
     template:
       metadata:
         labels:
           app: nginx
       spec:
         containers:
           - name: nginx
             image: nginx:1.25
             ports:
               - containerPort: 80
             readinessProbe:
               httpGet:
                 path: /
                 port: 80
               initialDelaySeconds: 5 # Wait 5s before starting checks
               periodSeconds: 5 # Check every 5s
               failureThreshold: 3 # After 3 failures → pod marked Unready
               timeoutSeconds: 2 # Timeout for each check
             livenessProbe:
               httpGet:
                 path: /
                 port: 80
               initialDelaySeconds: 10 # Wait 10s before checking liveness
               periodSeconds: 10 # Check every 10s
               failureThreshold: 3 # After 3 failed checks → pod restarted
               timeoutSeconds: 2 # Timeout for each check
   ```

9. You can deploy this YAML using

   ```sh
    kubectl apply -f <filename>.yaml.
   ```

10. Observe the log output to understand how probes checks periodically.

    ```sh
    kubectl logs -f <pod-name>
    ```
