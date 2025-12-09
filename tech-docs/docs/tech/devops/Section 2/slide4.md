---
sidebar_position: 4
sidebar_label: K8's Deployments & Namespaces
---

# Deployments

1. A `Deployment` is a higher level abstraction that manages a set of identical pods.
2. It ensures that a specified number of pod replicas are running at any given time.
3. If a pod goes down or crashed, the Deployment automatically creates a new pod
   to replace it, ensuring high availability.
4. Deployments also split pods across multiple nodes for fault tolerance.
5. You can also use Deployments to perform rolling updates to your application,
   allowing you to update your application without downtime.
6. Deployment also help us with rollout undo capabilities in case of bad deployments.
7. The `selector` field in Deployment helps to identify the pods that belong
   to the Deployment. So the matching happens based on labels.

   ![k8s_deployment](assets/k8s_deployment.png)

8. Generally `replica sets` are not created directly, instead they are created
   and managed by Deployments.

## Exercise Deployment using Imperative Command

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

## Exercise Deployment using YAML

1. Let's create a deployment using declarative way using the below command

   ```sh
   kubectl create deployment nginx-deployment --image=nginx  --replicas=3
    --dry-run=client -o yaml > deploy.yaml
   ```

2. The above command creates a deployment yaml file named `deploy.yaml` with
   3 replicas of nginx pods.

3. Now lets reduce the replicas to 2 in the `deploy.yaml` file & delete the
   existing deployment using the below command

   ```sh
   kubectl delete deployment nginx-deployment
   ```

4. Now let's apply the deployment using the below command

   ```sh
    kubectl apply -f deploy.yaml
   ```

5. Now verify the deployment and pods are created and running using the below commands

   ```sh
    kubectl get pods -o wide
   ```

6. Adding deployment yaml for reference

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
     labels:
       app: nginx-deployment
   spec:
     replicas: 2
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

## K8's Namespace

1. So far we deployed all our k8's resources in the `default` namespace.
2. Namespaces are like multiple teams within the same organization. Namespaces
   helps to create multiple virtual clusters within the same physical cluster.
3. Namespaces provide a scope. You can use namespaces to separate environments
   between the different teams within the same cluster.
4. You can create a namespace using the below command:

   ```sh
   kubectl create namespace <namespace-name>
   ```

5. You can see all the namespaces in the cluster using the below command:

   ```sh
   kubectl get namespaces
   ```

   ![k8's_namespace_1](assets/k8's_namespace_1.png)

6. For this workshop, we can use the `default` namespace. But in production,
   it's a good practice to create separate namespaces for different teams
   or projects.

7. To deploy resources in a specific namespace, you can use the `-n` or
   `--namespace` flag with kubectl commands. For example:

```sh
kubectl get pods -n <namespace-name>
```

## Summary

1. A Deployment is a higher level abstraction that manages a set of identical pods.
2. It ensures high availability by automatically replacing crashed pods
   and splitting pods across multiple nodes.
3. Deployments also support rolling updates and rollback capabilities.
4. Namespaces help to create multiple virtual clusters within the same physical cluster,
   providing isolation between different teams or projects.
