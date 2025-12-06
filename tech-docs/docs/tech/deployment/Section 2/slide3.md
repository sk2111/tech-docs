---
sidebar_position: 3
sidebar_label: Diving Deep into K8's
---

# Diving Deep into K8's

In the previous section we saw a high level overview of k8's architecture
& also pods.

But pods are just one of the many components in k8's. And like we discussed
in docker section, pods cannot solve all the problems of deployment.

For example pods are ephemeral in nature & if a pod goes down due to some reason,
there is no way to bring it back up again. Also pods do not provide features like
scaling.

To solve these problems, k8's provides higher level abstractions on top of pods
like `ReplicaSets, Deployments, Services, StatefulSets, Storage etc.,`

## Deployments

1. A `Deployment` is a higher level abstraction that manages a set of identical pods.
2. It ensures that a specified number of pod replicas are running at any given time.
3. If a pod goes down or crashed, the Deployment automatically creates a new pod
   to replace it, ensuring high availability.
4. Deployments also split pods across multiple nodes for fault tolerance.
5. You can also use Deployments to perform rolling updates to your application,
   allowing you to update your application without downtime.
6. Deployment also help us with rollout undo capabilities in case of bad deployments.
7. Generally `replica sets` are not created directly, instead they are created
   and managed by Deployments.
8. The `selector` field in Deployment helps to identify the pods that belong
   to the Deployment. So the matching happens based on labels.

![k8s_deployment](assets/k8s_deployment.png)

### Exercise Deployment using Imperative Command

1. First let' clean up the previous pod

   ```sh
   kubectl delete pod nginx-pod-1
   ```

   ```sh
   kubectl delete pod --all
   ```

2. Let's create a deployment using imperative command

   ```sh
    kubectl create deployment nginx-deployment --image=nginx  --replicas=3
   ```

3. Now verify the deployment and pods are created and running using the below commands

   ```sh
   kubectl get pods -o wide
   ```

   ![k8s_deployment_1](assets/k8's_deployment_1.png)

4. Now lets delete one of the pods and see if deployment brings it back up

   ```sh
   kubectl delete pod <pod-name>
   ```

5. Now verify the pods again using the below command & this proves that deployment
   controller is bringing back the desired state

   ```sh
   kubectl get pods -o wide
   ```

   ![k8s_deployment_2](assets/k8's_deployment_2.png)

6. You can see the pods are spread across multiple nodes for fault tolerance.

   1. Note this depends on the number of nodes in your cluster.
   2. If you have only one node, all pods will be scheduled on the same node.
   3. Also if there are not enough resources (CPU or RAM) in the nodes, pods will
      be scheduled on the same node.

7. Now let run this command

   ```sh
   kubectl get deployments
   ```

   ```sh
   kubectl describe deployment <name>
   ```

8. One thing to note here is if we need to delete the pod, we either edit the deployment
   to reduce the number of replicas or delete the deployment itself.

   ```sh
    kubectl scale deployment <name> --replicas=2
   ```

   ```sh
   kubectl delete deployment <name>
   ```

### Exercise Deployment using YAML

1. Let's create a deployment using declarative way using the below command

   ```sh
   kubectl create deployment nginx-deployment --image=nginx  --replicas=3
   --dry-run=client -o yaml > deploy.yaml
   ```

2. The above command creates a deployment yaml file named `deploy.yaml` with
   3 replicas of nginx pods.

3. Now let's apply the deployment using the below command

   ```sh
    kubectl apply -f deploy.yaml
   ```

4. Now verify the deployment and pods are created and running using the below commands

   ```sh
    kubectl get pods -o wide
   ```

5. Adding deployment yaml for reference

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
     labels:
       app: nginx-deployment
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: nginx-deployment
     template:
       metadata:
         labels:
           app: nginx-deployment
       spec:
       containers:
         - image: nginx
           name: nginx
   ```
