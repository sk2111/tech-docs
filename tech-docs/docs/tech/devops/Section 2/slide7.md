---
sidebar_position: 7
sidebar_label: Practice Time
---

# Practice Time

Now that we have understood the basic concepts of k8's, its time to practice
what we have learned so far.

## Prerequisites

1. Make sure you have `kubectl` and `minikube` installed in your local machine.
2. Make sure minikube cluster is up and running.
3. Make sure you have the node app or python app setup from previous section 1 practice.

## Let's Practice

1. Go to the python or node js app folder from previous section 1 practice.
2. Verify whether you logged in to the docker hub using the below command

   ```sh
   docker login
   ```

3. Build the docker image using the below command

   ```sh
   docker build -t <your-dockerhub-username>/<image-name>:<tag> .
   ```

   Example:

   ```sh
   docker build -t sathish1996/node-app:1.0.0 .
   ```

4. Push the docker image to docker hub using the below command

   ```sh
   docker push <your-dockerhub-username>/<image-name>:<tag>
   ```

   Example:

   ```sh
   docker push sathish1996/node-app:1.0.0
   ```

5. Now create a deployment yaml file `deployment.yaml` & `service.yaml` file &
   use the uploaded docker image in the deployment file.

   deployment.yaml

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: backend
     labels:
       app: backend
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: backend
     template:
       metadata:
         labels:
           app: backend
       spec:
         containers:
           - image: sathish1996/node-app:1.0.0
             name: backend
             ports:
               - containerPort: 4000
   ```

   service.yaml

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: backend
     labels:
       app: backend
   spec:
     selector:
       app: backend
     type: NodePort
     ports:
       - port: 4000
         protocol: TCP
         targetPort: 4000
         nodePort: 30080
   ```

6. Apply the deployment using the below command

   ```sh
    kubectl apply -f deployment.yaml
   ```

7. Apply the service using the below command

   ```sh
    kubectl apply -f service.yaml
   ```

8. Verify the deployment & pods are created and running using the below commands

   ```sh
    kubectl get deployments

    kubectl get pods -o wide
   ```

9. Expose the service using the below command

   ```sh
   minikube service backend
   ```

10. If everything is done correctly you should be able to see the app running in
    the browser.

11. Watchout for common issues like `image pull issues, wrong image name/tag,
container port is correctly mapped with the service target port`.

12. Now lets create a busybox pod to test the network connectivity to the
    backend service.

    Create a busybox pod using the below command

    ```sh
    kubectl run busybox --image=busybox --command -- sleep 3600
    ```

    Exec into the busybox pod using the below command

    ```sh
    kubectl exec -it busybox -- sh
    ```

    Now inside the busybox pod use wget or curl to test the connectivity to the
    backend service using the service name and port.

    ```sh
    wget -qO- http://backend:4000
    ```

    You should be able to see the response from the backend service & this shows
    that the service discovery is working fine.

13. Congrats you have successfully deployed your first app in k8's cluster!

:::tip

1. kubernetes official documentation is great place to learn more
   [kubernetes.io](https://kubernetes.io/docs/)
2. Quick Reference sheet for kubectl commands
   [look here](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

:::
