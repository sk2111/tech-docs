---
sidebar_position: 4
sidebar_label: Docker Networks & Volumes
---

# Docker Networks & Volumes

In this section, we will explore two important concepts in Docker

1. **Docker Networks** - how containers communicate
2. **Docker Volumes** - how containers persist data

Both are essential for building real-world applications with
multiple containers (database + backend + frontend) need to
communicate and persist data safely.

## Docker Networks

Docker networks allow containers to

1. Communicate with each other.
2. Communicate with the outside world
3. Stay isolated from containers they should not interact with

By default, every container is attached to a specific network type.

## Types of Docker Networks

Docker supports multiple network drivers, each designed for a different use case.

### 1. `bridge` (Default)

A private internal network created by Docker on the host machine.

1. Containers get an internal IP
2. Containers can talk to each other using container names & IP's
3. Default bridge network only **allows communication between containers via IP addresses,
   not container names**
4. Best suited for **local development** and **single-host setups**

**Example:**

```sh
docker network ls

docker network create my-bridge

docker run --network my-bridge -d --name c1 busybox sleep 300

docker run --network my-bridge -d --name c2 busybox sleep 300

docker exec -it c1 ping c2
```

![Docker Bridge Network](./assets/docker_bridge.png)

### 2. `host`

The container shares the host machine’s network stack.

1. No isolation between host and container
2. Useful when container needs high network performance
3. Best for apps using custom network drivers, VPN tools, monitoring agents, etc.

**Example:**

```sh
docker run --network host nginx
```

### 3. `none`

The container has no network interface except the loopback.

1. No internet
2. No communication & fully isolated
3. Useful for security or batch jobs

**Example:**

```sh
docker run --network none busybox ifconfig
```

### 4. `overlay`

1. Allows containers running on different hosts to communicate
2. Used for distributed microservices
3. Automatically encrypted depending on config

---

## Docker Volumes

Containers are ephemeral - when a container is removed, its internal data is lost.
Volumes help solve this by providing persistent storage.

1. Persist data beyond container lifecycle
2. Share data between containers
3. Decouple data from the container image

Commonly used for databases, logs, user uploads, etc.

## Types of Docker Volumes

### 1. Named Volumes

Managed by Docker.

**Example:**

```sh
docker volume create mydata

docker volume inspect mydata

docker run -it -v mydata:/test busybox sh

echo "Hello, Volume!" > /test/hello.txt

exit

docker run -it -v mydata:/test busybox sh

cat /test/hello.txt
```

### 2. Bind Mounts

Mounts a file or directory from the host machine into the container.
**Example:**

```sh
mkdir -p /path/on/host

echo "Hello from Host!" > /path/on/host/hostfile.txt

docker run -it -v /path/on/host:/data busybox sh

cat /data/hostfile.txt

echo "Hello from Container!" > /data/containerfile.txt

exit

cat /path/on/host/containerfile.txt
```

:::tip[Production Tip]

:::

## Best Practices

1. Use **Named Volumes** for most cases - easier to manage, portable, storage
   maintained by Docker and optimized for Docker.
2. Regularly back up important volumes to avoid data loss for critical data.
3. Use **Bind Mounts** for local development - allows syncing code between host
   and container.
4. Isolate sensitive data using separate volumes with proper permissions.
5. Use network to isolate containers that don't need to communicate.

##

---

## Summary

1. Docker Networks enable container communication and isolation.
2. Multiple network types exist for different use cases: bridge, host, none, overlay.
3. Docker Volumes provide persistent storage for containers.
4. Named Volumes are managed by Docker and are preferred for most scenarios.
5. Bind Mounts link host files/directories to containers, useful for development.
6. Follow best practices for managing networks and volumes in production environments.
