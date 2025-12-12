---
sidebar_position: 3
sidebar_label: K8's StatefulSets
---

# Kubernetes StatefulSets - Managing State in K8's

1. In Kubernetes, most workloads are stateless, meaning they don't retain any
   data between restarts.
2. We use `Deployments` for such stateless applications.
3. However, some applications require `persistent storage and stable network identities`.
4. Examples include databases like MySQL, PostgreSQL, and
   distributed systems like Kafka.
5. This is where **StatefulSets** come into play.
6. A StatefulSet is a Kubernetes resource that manages the deployment and scaling
   of stateful applications.
7. It provides guarantees about the ordering and uniqueness of pods.
8. Key features of StatefulSets:
   - Stable, unique network identifiers for each pod.
   - Stable, persistent storage using PersistentVolumeClaims (PVCs).
   - Ordered, graceful deployment and scaling of pods.
   - Ordered, automated rolling updates.
9. Here's an example of a StatefulSet definition for a simple MySQL database:

   ```yaml
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: mysql
   spec:
     serviceName: "mysql"
     replicas: 3
     selector:
       matchLabels:
         app: mysql
     template:
       metadata:
         labels:
           app: mysql
       spec:
         containers:
           - name: mysql
             image: mysql:5.7
   ```
