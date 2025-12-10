---
sidebar_position: 5
sidebar_label: K8's Recap
---

# Recap of K8's Concepts

1. We started with understanding the basic concepts of K8's like `Pods,
Deployments, Services, Namespaces` etc.
2. We understood the architecture of K8's and its components like `Master Node,
Worker Nodes, Kubelet, Kube-Proxy` etc.
3. We learned how to create and manage K8's resources using `kubectl` command &
   yaml files.
4. We explored how to pass environment variables to pods using `ConfigMaps` and `Secrets`.
5. We also saw how to manage application configuration using `ConfigMaps` and `Secrets`.
6. we learned how to create and use `Secrets` for sensitive data like
   passwords, API keys, etc.
7. We understood the importance of `resource requests` and `limits` in K8's.
8. We explored `Horizontal Pod Autoscaling (HPA)` and how it helps in scaling
   applications based on resource utilization.
9. We learned how K8's collects metrics using `Metrics Server`.

:::tip[Production tip]

1. In our production environments, we will use the combination of `ConfigMaps` and
   `Secrets` to manage application configuration and sensitive data.
2. We will set appropriate `resource requests` and `limits` for our pods to
   ensure optimal resource utilization and prevent resource contention.
3. We will implement `Horizontal Pod Autoscaling (HPA)` to automatically
   scale our applications based on resource utilization and traffic patterns.
4. We will deploy using `Deployments` to ensure high availability and
   easy management of our applications.
5. And expose only necessary services using `Services` to ensure security and
   accessibility of our applications.
6. In Short, in production your application will be `Secrets`, `ConfigMaps`,
   `Resource Management`, `HPA`, `Deployments` and `Services`.

:::
