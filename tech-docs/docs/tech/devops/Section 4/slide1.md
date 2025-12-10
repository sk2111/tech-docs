---
sidebar_position: 1
sidebar_label: K8's Volumes
---

# K8's Volumes

1. In K8's we have both `ephemeral` and `persistent` volumes.
2. Ephemeral volumes are temporary and are deleted when the pod is deleted.
3. Persistent volumes are long-lasting and can be reused by different pods &
   provide persistent storage.
4. Volumes allow data to persist beyond the lifecycle of a pod, enabling data
   sharing between containers in a pod and across pod restarts.
5. K8's supports various types of volumes like `emptyDir`, `hostPath`,
   `persistentVolumeClaim` etc.

## Ephemeral Volumes - emptyDir

1. `emptyDir` is a type of ephemeral volume that is created when a pod is
   assigned to a node and exists as long as the pod is running on that node.
2. Data in an `emptyDir` volume is stored on the node's filesystem and is
   deleted when the pod is removed from the node.
3. `emptyDir` volumes are useful for temporary storage of data that does not
   need to persist beyond the lifecycle of the pod.
4. Below is an example of how to use `emptyDir` volume in a pod specification.

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: emptydir-app
     labels:
       app: emptydir-app
   spec:
     volumes:
       - name: emptydir-volume
         emptyDir: {}
     containers:
       - name: busybox
         image: busybox
         command: ["sh", "-c", "sleep 3600"]
         volumeMounts:
           - name: emptydir-volume
             mountPath: /data
   ```

5. In the above example, we have created a pod named `emptydir-app` with an
   `emptyDir` volume named `emptydir-volume`. The container writes a file
   `hello.txt` to the `/data` directory, which is mounted to the `emptyDir`

6. Let's see how to create a pod with an `emptyDir` volume and verify its
   functionality.

   ```sh
   kubectl apply -f emptydir-pod.yaml
   ```

7. Once the pod is running, we can exec into the container and create a file
   in the `emptyDir` volume.

   ```sh
   kubectl exec -it emptydir-app -- sh
   echo "Hello, World!" > /data/hello.txt
   exit
   ```

8. Now, if we delete the pod and recreate it, we will see that the file
   `hello.txt` is no longer present, demonstrating the ephemeral nature of
   `emptyDir` volumes.

   ```sh
   kubectl delete pod emptydir-app
   ```

   ```sh
   kubectl apply -f emptydir-pod.yaml

   ```

   ```sh
   kubectl exec -it emptydir-app -- sh
   ls /data
   ```

## Persistent Volumes & Persistent Volume Claims

1. Persistent Volumes (PVs) are a way to provide persistent storage in K8's.
2. PVs are created by administrators.
3. Persistent Volume Claims (PVCs) are requests for storage by users. PVCs can
   specify size, access modes, and storage class.
4. It claims a piece of storage from a PV and binds to it.
5. K8's Controller manages the binding between PVs and PVCs.
6. Access modes define how the volume can be mounted (e.g., ReadWriteOnce,
   ReadOnlyMany, ReadWriteMany).
7. Both the PV and PVC must have compatible access modes for successful binding.
8. Example you cannot bind a PV with ReadWriteOnce access mode to a PVC
   requesting ReadOnlyMany access mode.

![k8s_pv_1](assets/k8s_pv_1.png)

## Static & Dynamic Provisioning

1. In static provisioning, an administrator creates a set of PVs that users
   can claim.
2. In dynamic provisioning, K8's automatically provisions a PV when a PVC is
   created, based on the storage class specified in the PVC.
3. Dynamic provisioning requires a `storage class` to be defined in the cluster.
4. When a PVC is created with a specific `storage class`, K8's uses the
   provisioner defined in that storage class to create a new PV that meets
   the PVC's requirements.
5. Storage classes contains the provisioner information and parameters needed for
   dynamic provisioning.
6. Provisioners like `kubernetes.io/aws-ebs`, `kubernetes.io/gce-pd`, etc. are
   used to create PVs on different cloud providers.
7. Storage classes has other parameters like `reclaim policy`, `volume binding mode`,
   etc. that define the behavior of the provisioned PVs.
8. Storage classes also defines the type of storage (e.g., SSD, HDD)

## Reclaim Policy

1. Now let's say you don't need a PVC anymore, what happens to it?
2. Reclaim policy defines what happens to a PV when its bound PVC is deleted.
3. Common reclaim policies are `Retain`, `Recycle`, and `Delete`.
4. `Retain` keeps the PV and its data intact for manual reclamation. Data is
   preserved.
5. `Delete` removes the PV and its data from the cluster. Data is lost.
   Not recommended for production.
6. The reclaim policy is set when the PV is created and can be modified later
   if needed.
