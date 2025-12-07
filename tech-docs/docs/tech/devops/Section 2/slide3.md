---
sidebar_position: 3
sidebar_label: Diving Deep into K8's
---

# Diving Deep into K8's

In the previous section we saw a high level overview of k8's architecture
& also pods.

But pods are just one of the many components in k8's. And like we discussed
in docker section, pods alone cannot solve all the problems.

For example if a pod goes down due to some reason,
there is no way to bring it back up again.

Also pods do not provide features like
scaling.

To solve these problems, k8's provides higher level abstractions on top of pods
like `ReplicaSets, Deployments, Services, StatefulSets, Storage etc.,`

To give you a brief overview of different concepts in k8's

1. Pods
2. Deployments
3. Services
4. ConfigMaps and Secrets
5. Volumes and Persistent Volumes
6. StatefulSets
7. Namespaces
8. Ingress Controllers
9. RBAC & Role Bindings
10. Cron Jobs
11. Daemon Sets
12. Jobs
13. Replication Controllers
14. Storage Classes
15. Network Policies
16. And many more..

:::tip
If you are interest in doing kubernetes certification, please refer to the official
[Kubernetes training website](https://kubernetes.io/training/) for more details.
:::
