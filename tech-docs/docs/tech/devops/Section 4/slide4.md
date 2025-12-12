---
sidebar_position: 4
sidebar_label: K8's Good to know
---

# Kubernetes Good to know - Additional Concepts

## K8's Job & CronJob

1. In Kubernetes, a `Job` is used to run a task to completion.
2. It ensures that a specified number of pods successfully terminate.
3. Once the task is completed, the pods are not restarted.
4. A `CronJob` is used to run jobs on a scheduled basis, similar to cron jobs in
   Unix/Linux.
5. You can define the schedule using standard cron syntax.
6. Here's an example of a Job definition:

   ```yaml
   apiVersion: batch/v1
   kind: Job
   metadata:
     name: example-job
   spec:
     template:
       spec:
         containers:
           - name: example
             image: busybox
             command: ["echo", "Hello, World!"]
         restartPolicy: Never
     backoffLimit: 4
   ```

7. And here's an example of a CronJob definition:

   ```yaml
   apiVersion: batch/v1
    kind: CronJob
    metadata:
      name: example-cronjob
    spec:
      schedule: "*/5 * * * *" # Runs every 5 minutes
        jobTemplate:
            spec:
            template:
                spec:
                containers:
                    - name: example
                    image: busybox
                    command: ["echo", "Hello from CronJob!"]
                restartPolicy: Never
   ```

## K8's DaemonSets

1. A `DaemonSet` ensures that a copy of a pod runs on all (or some) nodes in a
   Kubernetes cluster.
2. It is typically used for deploying system-level services like log collectors,
   monitoring agents, or network plugins.
3. When new nodes are added to the cluster, the DaemonSet automatically
   schedules pods on those nodes.
4. Here's an example of a DaemonSet definition:

   ```yaml
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: example-daemonset
    spec:
      selector:
        matchLabels:
          app: example
        template:
            metadata:
                labels:
                app: example
            spec:
                containers:
                - name: example
                    image: busybox
                    command: ["sh", "-c", "while true; do echo Hello from DaemonSet; sleep 60; done"]
   ```

## K8's Tolerations & Affinities

1. Tolerations and Affinities are used to control pod scheduling in Kubernetes.
2. Tolerations allow pods to be scheduled on nodes with specific taints.
3. Affinities allow you to define rules for pod placement based on node labels
   and other criteria.
4. Here's an example of using tolerations and affinities in a pod definition:

   ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: example-pod
    spec:
      containers:
        - name: example
          image: busybox
          command: ["sh", "-c", "while true; do echo Hello from Pod; sleep 60; done"]
        tolerations:
        - key: "key1"
          operator: "Equal"
          value: "value1"
          effect: "NoSchedule"
        affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: "disktype"
                operator: In
                values:
                - ssd
   ```

5. In this example, the pod will tolerate nodes with the specified taint and
   will only be scheduled on nodes with the label `disktype=ssd`.

## K8's Roles & RoleBindings

1. In Kubernetes, `Roles` and `RoleBindings` are used to manage access control
   within a namespace.
2. A `Role` defines a set of permissions (like read, write, delete) for
   resources within a specific namespace.
3. A `RoleBinding` associates a `Role` with a user or a group, granting them
   the permissions defined in the `Role`.
4. Here's an example of a Role definition:

   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     namespace: default
     name: pod-reader
   rules:
     - apiGroups: [""]
       resources: ["pods"]
   ```
