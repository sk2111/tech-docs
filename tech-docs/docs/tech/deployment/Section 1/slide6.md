---
sidebar_position: 6
sidebar_label: Evolution of Deployment
---

# Evolution of Deployment

Now that you have learned the basics of Docker, Dockerfile and Docker Compose,
it's time to see how everything fits together.

When someone starts their container journey, everything begins simple.
You have an application. You package it in a container. You run it with Docker.
Life is good!

But as the application grows, the needs grow too.
This is the natural evolution that any team go through.

## 1. The Beginning: single Container with Docker

A beginner starts with:

```sh
docker build -t myapp .
docker run -p 3000:3000 myapp
```

This works perfectly for local development, simple applications & quick demos.

### Limitation appears

What if you have multiple services? A backend, frontend, database, Redis, message
queue etc., running with 10 docker run commands becomes unmanageable.

## 2. Growing Up: Multi-Container Apps with Docker Compose

The developer now organizes everything using a `docker-compose.yml`.

With just one command

```sh
docker compose up -d
```

1. They get multiple services, networking out-of-the-box, volumes for data,
   easy environment variable management

2. At this stage, many real-world projects stop here and there’s nothing wrong
   with that

3. If your app is small/medium and runs on a single machine, App Service, VM or EC2,
   Docker Compose is enough and it’s a perfect solution.

But then, the project grows again…

## 3. The Demand for Scaling: Enter Docker Swarm

The team now faces problems

1. They want multiple VMs/nodes to cope with increased traffic.
2. They need load balancing.
3. They want failover when a container crashes.
4. They want rolling updates without downtime.

Docker Compose cannot do this. Docker Swarm acts as the middle ground between
simplicity and scalability.

### What Docker Swarm Gives You

1. Multi-node cluster (managers + workers)
2. Service scaling
3. Built-in load balancing
4. Rolling updates & rollbacks
5. Secrets management

Example Swarm deployment

```sh
docker swarm init

docker stack deploy -c docker-compose.yml myapp
```

Swarm uses the same Docker Compose syntax, so the learning curve is low.

### Architecture of Docker Swarm

![Docker Swarm Architecture](./assets/docker_swarm_architecture.webp)

:::important
However, as systems become more complex having lot of microservices, distributed
systems, enterprise SLAs Docker Swarm eventually hits limits.
:::

## 4. The Final Evolution: Kubernetes (K8's)

The organization now wants

1. Auto-scaling based on load
2. Self-healing infrastructure
3. Advanced networking policies
4. Ingress controllers
5. Observability at scale
6. Canary deployments & blue-green releases

This is where Kubernetes enters.

`Kubernetes is not a replacement for Docker or container runtimes,
it's an orchestration system that runs containers at massive scale.`

With Kubernetes, you get

1. Cluster-level scheduling
2. Horizontal Pod Autoscaling (HPA)
3. Node health checks & self-healing
4. Service discovery & load balancing
5. ConfigMaps & Secrets
6. Rollout's & rollbacks
7. Enterprise-grade extensions (Istio, Prometheus, ArgoCD, etc.)

This is why nearly every enterprise company chooses Kubernetes for

1. Enterprise scale
2. High availability
3. Multi-region deployments
4. Cloud-native architectures

## Final Takeaway

There is no `wrong choice.` You choose the tool based on your project’s needs.

1. If small project & one VM + few services → Docker Compose
2. If simple to medium project requiring multi-node → Docker Swarm
3. If large-scale, enterprise, microservices → Kubernetes

:::tip

1. Docker swarm is still catching up with features compared to Kubernetes.
2. At present, its a good middle ground for small to medium projects without
   Kubernetes complexity.

:::
