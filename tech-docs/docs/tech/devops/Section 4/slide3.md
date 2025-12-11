---
sidebar_position: 3
sidebar_label: K8's Zero to Hero Exercise
---

# K8's Zero to Hero - The Final Exercise

1. You did it ! You have completed the K8's fundamentals.
2. Now you have all the knowledge required to deploy a full stack application on
   Kubernetes.
3. Your task is to deploy a simple web application that uses a frontend, backend
   and a database (Postgres or Redis)
4. The frontend need to communicate with the backend, and the backend needs to
   communicate with the database.
5. It can be a simple application like a TODO app, a blog or any other web
   application of your choice.
6. The main goal of this exercise is to test your understanding of K8's concepts
   and your ability to deploy and manage applications on K8's.
7. You need to create the necessary `deployments, services,
Secrets, ConfigMaps, HPA, resource limits, persistent volumes etc.,`.
8. You can use any web application of your choice, but make sure it has a
   frontend, backend, and a database component.
9. Good luck and happy deploying!

:::tip[Hints]

1. Use `ConfigMaps` to pass configuration data to your application.
2. Use `Secrets` to pass sensitive data like database passwords.
3. Set appropriate `resource requests` and `limits` for your pods.
4. Implement `Horizontal Pod Autoscaling (HPA)` to scale your application based
   on resource utilization.
5. Use `Persistent Volumes` to store data for your database.
6. Expose your frontend using a `Service` of type `LoadBalancer` or `NodePort`.
7. Refer to the previous sections for examples and guidance on how to create
   each component.
8. You can use single yaml file with multiple documents separated by `---` & have
   all your deployment, services etc., or multiple yaml files as per your preference.
9. **Optional** - You can also try deploying using `Helm Charts` for
   better management. Read about Helm in the [Helm Docs](https://helm.sh/docs/).

## Example Architecture

Web application with frontend, backend & database

![k8s_hero](assets/k8s_hero.png)

:::

:::tip[minkube]

1. minikube comes with dashboard which you can use to visualize your deployments, pods, services etc.,
2. To access the dashboard, run `minikube dashboard` command in your terminal,

:::
