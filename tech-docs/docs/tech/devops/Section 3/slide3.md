---
sidebar_position: 2
sidebar_label: K8's Secrets
---

# K8's Secrets

1. In K8's, Secrets are used to store sensitive information such as passwords,
   OAuth tokens, and ssh keys.
2. Secrets are similar to ConfigMaps but are specifically designed to hold
   confidential data.
3. Secrets are stored in an encoded format (base64) to provide a basic level of security.
4. We have multiple types of secrets like Opaque, Docker Registry, TLS, basic,
   ssh, k8's token etc.
5. **Important**: While Secrets provide a basic level of security by encoding
   the data, they are not encrypted by default.

:::important

1. With native K8's **secrets are not encrypted by default**.
2. For enhanced security, consider using additional encryption mechanisms or
   tools like K8's Encryption at Rest or external secret management solutions.
3. Always follow best practices for managing sensitive data in your applications.
4. In real projects, we store secrets in external secret management tools
   like HashiCorp Vault, AWS Secrets Manager, Azure Key Vault,
   Github Secrets, etc., and inject them into K8's at runtime.
5. Managed K8's services like GKE, EKS, AKS provide integration with
   their respective secret management services for better security.

:::

## Create a Opaque secret

1. Opaque type Secrets can be created using yaml files.

   ```yaml
   apiVersion: v1
   kind: Secret
   metadata:
     name: app-secret
   type: Opaque
   data:
     # base64 encoded value of 'password123' injected from external secret
     # manager in production
     DB_PASSWORD: cGFzc3dvcmQxMjM=
     # base64 encoded value of 'apikey123456'
     # manager in production
     API_KEY: YXBpa2V5MTIzNDU2
   ```

2. Apply the secret using the below command

   ```sh
    kubectl apply -f secret.yaml
   ```

3. Verify whether the secret is created using the below command

   ```sh
    kubectl get secrets
   ```

4. Now update the deployment yaml to use the secret as shown below

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
             imagePullPolicy: Always
             name: backend
             ports:
               - containerPort: 4000
             envFrom:
               - configMapRef:
                   name: app-config
               - secretRef:
                   name: app-secret
   ```

5. This will pass all the key value pairs from the secret as environment variables
   to the pod.
6. Apply the deployment using the below command

   ```sh
    kubectl apply -f deployment.yaml
   ```

7. Verify the pods are recreated using the below command

   ```sh
    kubectl get pods
   ```

8. Use the below command to exec into the pod and verify the secret values are
   available as environment variables

   ```sh
    kubectl exec -it <pod-name> -- sh

    printenv
   ```

   ```sh
    minikube kubectl -- exec -it <pod-name> -- sh

    printenv
   ```

![k8s_secret_5](./assets/k8s_secret_5.png)
