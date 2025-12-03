# Containerization

DevOps is a set of practices, tools, and a cultural philosophy that automates and
integrates the processes between software development (Dev) and IT operations
(Ops) teams

Containerization concept is the foundation of modern DevOps.

## Why Containers in a DevOps?

1. Modern DevOps cannot be explained without understanding `containerization`.
2. Cloud deployments, microservices, CI/CD pipelines all rely heavily on this concept.
3. `Containers ≠ Docker`. Docker is just one container engine. Others include
   `containerd, CRI-O, Podman, LXC`
4. Cloud platforms (AWS, Azure, GCP) internally run their services using container
   technologies.

## Back to history - The Old Deployment World

1. Imagine a powerful physical server: 12 cores, 32 GB RAM.
2. You deploy one Java or Node or Python app on it.
3. The app uses maybe 20-30% CPU and 10% RAM.
4. You could definitely deploy another app, but
   1. Conflicts in packages, runtime versions, ports, etc.
   2. If one app crashes, it can sometimes affect the entire system.
   3. Security isolation is weak.
5. This leads to **huge resource wastage**

`This problem remained for years:`
**`How to better utilize a single machine safely ?`**

## The first solution: Hypervisors & Virtual Machines (VM's)

A hypervisor `(VMware, Hyper-V, KVM, VirtualBox)` is a special management layer that

1. Sits on top of raw hardware
2. Slices CPU, RAM, Disk, Network & more
3. Creates fully isolated Virtual Machines

![Hypervisor](./assets/hypervisor.png)
![Virtual Machine](./assets/virtual_machine.png)

### Benefits

1. Strong isolation - each VM has its own full OS
2. Different apps can run different OS versions
3. Safer and more flexible than just installing apps directly on the host

### Limitations

1. Each VM runs its own full OS - heavy (GBs of RAM needed per VM)
2. Slow to start (minutes)
3. Poor density - fewer VMs per server

`This made people ask the next big question:`
**`How do we isolate apps like VM's, but make it super lightweight?`**

## The second evolution: Containers

Instead of packaging a full OS, containers reuse the host OS kernel.
They isolate only

1. File system
2. Processes
3. Network
4. Resources

This is possible using Linux features like `namespaces` and `cgroups`.

![Docker Engine](./assets/docker_engine.png)

:::tip
Learn more about [Linux kernel namespaces and cgroups](https://blog.nginx.org/blog/what-are-namespaces-cgroups-how-do-they-work)
:::

## Docker's contribution

Docker didn’t invent containerization &
Linux containers existed earlier `(LXC, Solaris Zones, BSD Jails)`.

### But Docker made containers

1. Easy to build
2. Easy to ship
3. Easy to run & portable
4. Easy to share (Docker Hub)

## Why containers became a game changer

1. Extremely lightweight & consistent
2. Start in seconds
3. Easy to scale, portable & efficient
4. Cloud platforms adopted them rapidly

### When to choose VM's?

1. You need strong isolation
2. You require different operating systems on the same hardware
3. You need to run legacy applications that require a full OS environment
4. You want to run applications that need direct access to hardware or specialized
   drivers

### When to choose Containers?

1. You want lightweight and fast application deployment
2. You need to scale applications quickly and efficiently
3. You are developing cloud-native or microservice applications
4. You want to maximize resource utilization on a single host
