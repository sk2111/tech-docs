---
sidebar_position: 7
sidebar_label: K8's Env & Config Maps
---

# K8's Env

1. In any application, we have certain configuration values that are required for the application to run.
2. These configuration values are generally passed as environment variables.
3. In K8's, we can pass environment variables to our pods using the `env` field in the pod specification.
4. In the case of `deployment` we can pass environment variables in the same way as we do in pods.

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
        name: node-app
        labels:
            app: node-app
    spec:
        replicas: 3
        selector:
            matchLabels:
                app: node-app
        template:
            metadata:
                labels:
                    app: node-app
            spec:
                containers:
                    - image: sathish1996/node-app:1.1.1
                      name: node-app
                      ports:
                        - containerPort: 4000
                      env:
                        - name: APP_NAME
                          value: "I am awesome node app"
                        - name: LOG_LEVEL
                          value: "DEBUG"
    ```

## Practice Time - Env Variables

1. You can edit the python app or node js app from previous practice to read the environment variable `APP_NAME` and return it in the response.

2. Rebuild the docker image and push it to docker hub.
3. Update the deployment yaml file to include the env variable as shown above.
4. Apply the deployment using the below command

   ```sh
    kubectl apply -f deployment.yaml
   ```

5. Verify the pods are recreated using the below command

   ```sh
    kubectl get pods
   ```

6. Access the application using the service & minikube service URL & verify the response contains the environment variable value.

7. Tip: To debug the environment variables in the pod, you can exec into the pod and run the `printenv` command.

   ```sh
    kubectl exec -it <pod-name> -- sh
    printenv
   ```

   ```sh
    minikube kubectl -- exec -it <pod-name> -- sh
    printenv
   ```

![k8s_env_1](./assets/k8s_env_1.png)

## K8's Config Maps
