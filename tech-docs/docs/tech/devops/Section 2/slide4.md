---
sidebar_position: 4
sidebar_label: Diving Deep into K8's World
---

# Diving into K8's World

1. In the previous section we saw a high level overview of k8's architecture
   & also pods. But pods are just one of the many concepts in k8's.
2. And like we discussed in earlier section, pods alone cannot solve all the real
   world problems. For example if a pod goes down due to some reason,
   there is no way to bring it back up again. Also pods do not provide features like
   scaling.
3. To solve these problems, k8's provides higher level abstractions (concepts) on
   top of pods like `ReplicaSets, Deployments, Services, StatefulSets, Storage etc.,`

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

Each of these concepts solves a specific problem and helps you manage your
k8's cluster effectively.

:::tip[Certification on K8's]
If you are interest in doing kubernetes certification, please refer to the official
[Kubernetes training website](https://kubernetes.io/training/) for more details.
:::
