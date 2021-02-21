# Chapter 5 - Pods

Why - colocate multiple applications into a single atomic unit, scheduled onto a single machine

Ex. - Web Serving Container & Git Sync Container with a shared filesystem

Resource isolation - helps prevent one app stealing the memory of another 

Doesn't make sense for containers to be on separate machines, so they are grouped into a single unit - *a pod* (a pod is a group of whales, also)

## Pods in Kubernetes

Pod
  - Represents a collection of application containers and volumes running in the same execution environment
  - Smallest deployable artifact in a k8s cluster
  - All containers in a Pod always land on the same machine
  - Each container within a pod runs in its own cgroup, but they share a number of Linux namespaces
  - Apps in same pod share the same IP address and port space (network namespace), have the same hostname (UTS namespace), and can communicate using native interprocess communication channels over System V IPC or POSIX message queues (IPC namespace)
  - Apps in different Pods are isolated from each other; they have different IP addresses, different hostnames, and more

## Thinking with Pods

Ex. WorkPress app + database - not symbiotic and don't scale as a unit

Ask yourself, "Will these containers work correctly if they land on different machines?"
  - If no, a Pod is the correct grouping
  - If yes, multiple Pods is probably the correct solution

## The Pod Manifest

Pod manifest - text file representation of the k8s API object

k8s strongly believes in *declarative configuration* - write down desired state & submit to a service that ensures the desired state becomes the actual state

Declarative configuration > imperative configuration

k8s API server accepts and proccesses Pod manifests before storing them in persistent storage (`etcd`)

Scheduler uses k8s API to find Pods that haven't been scheduled toa node

Scheduler puts Pods onto nodes depending on resources and other constraints expressed in the Pod manifest

Multiple pods can be placed on the same machine as long as there are suffficient resources. However, scheduling multiple replicas of the same application onto the same machine is worse for reliability, since the machine is a single failure domain. Consequently, the Kubernetes scheduler tries to ensure that Pods from the same application are distributed onto different machines for reliability in the presence of such failures.

Once scheduled to a node, Pods don’t move and must be explicitly destroyed and rescheduled.

ReplicaSets (ch. 9) are better suited for running multiple instances of a Pod (and a single Pod too)

### Creating a Pod

Create Pod:

``` 
kubectl run kuard --generator=run-pod/v1 \
  --image=gcr.io/kuar-demo/kuard-amd64:blue
```

See status of Pod:

`kubectl get pods`

Delete a Pod:

`kubectl delete pods/kuard`

### Creating a Pod Manifest

Can use YAML or JSON, but YAML is preferred

Treat manifests like source code (e.g. comments to help explain what's going on)

Manifests contain a few key attributes:
  - `metadata` - describes the Pod and its labels
  - `spec` - describes volumes & lists containers that will run in the pod

Deployed `kuard` in ch 2:

```
docker run -d --name kuard \
  --publish 8080:8080 \
  gcr.io/kuar-demo/kuard-amd64:blue
```

Ex. `kuard-pod.yaml`

## Running Pods

Launch a single instance:

`kubectl apply -f kuard-pod.yml`

Manifest is submitted to k8s API server -> Pod is scheduled on healthy node in cluster

### Listning pods

`kubectl get pods`

Aside - getting more info from `kubectl`
  - `-o wide` will print out more info for a command
  - `-o json` or `-o yaml` will print out their respective objects

### Pod Details

To find out more about a Pod or any k8s object use `kubectl describe`

`kubectl describe pods kuard`

### Deleting a Pod

`kubectl delete pods/kuard`

Or use same file you used to create it:

`kubectl delete -f kuard-pod.yaml`

Deleted pods are not immediately killed; they have a *grace period* (30 seconds by default)

Pods in the `Terminating` state do not recieve new requests

When a Pod is deleted, any data stroed in the containers associated with that Pod will be deleted as well. If you want to persist data across multiple instances of a Pod, you need to use `PersistentVolumes`

## Accessing Your Pod

### Using Port Forwarding

`kubectl port-forward kuard 8080:8080`

^ Creates a secure from your local machine, through the Kubernetes master, to the instance of the Pod running on one of the worker nodes

### Getting More Info with Logs

`kubectl logs kuard`

^ Downloads logs from the running instance. Adding `-f` will continuously stream logs. Adding `--previous` will get logs from the previous instance

### Running Commands in Your Container with exec

`kubectl exec kuard date`

`kubectl exec -it kuard ssh` - gets interactive session

### Copying Files to and from Containers

From container:

`kubectl cp <pod-name>:/captures/capture3.txt ./capture3.txt`

To container:

`kubectl cp $HOME/config.txt <pod-name>:/config.txt`

Copying files to a container is an anti-pattern. You should treat the contents of a container as immutable

## Health Checks

k8s has health checks for application *liveness* - verify that an app is not just still running, but is also functioning properly. Usually have to be defined in Pod manifest since they are application-specific

### Liveness Probe

Example - The preceding Pod manifest uses an httpGet probe to perform an HTTP GET request against the /healthy endpoint on port 8080 of the kuard container. The probe sets an initialDelaySeconds of 5, and thus will not be called until 5 seconds after all the containers in the Pod are created. The probe must respond within the 1-second timeout, and the HTTP status code must be equal to or greater than 200 and less than 400 to be considered successful. Kubernetes will call the probe every 10 seconds. If more than three consecutive probes fail, the container will fail and restart

Running:

```
kubectl apply -f kuard-pod-health.yaml
kubectl port-forward kuard 8080:8080
```

### Readiness Probe

Liveness - is an app running properly?
Readiness - is a container ready to serve user requests?

Containers that fail readiness checks are removed from service load balancers

Readiness probes are configured similarly to liveness probes

### Types of Health Checks

In addition to HTTP checks, Kubernetes also supports tcpSocket health checks that open a TCP socket; if the connection is successful, the probe succeeds. This style of probe is useful for non-HTTP applications; for example, databases or other non–HTTP-based APIs

Finally, Kubernetes allows exec probes. These execute a script or program in the context of the container. Following typical convention, if this script returns a zero exit code, the probe succeeds; otherwise, it fails. exec scripts are often useful for custom application validation logic that doesn’t fit neatly into an HTTP call

## Resource Management

*Utilization* metric - amount of a resource actively being used divided by the amount of a resource that has been purchased

k8s can drive utilization to greater than 50%

Resource *requests* specify the minimum amount of a resource required to run the app

Resource *limits* specify the max amount that an app can consume

### Resource Requests: Minimum Required Resources

With Kubernetes, a Pod requests the resources required to run its containers. Kubernetes guarantees that these resources are available to the Pod. The most commonly requested resources are CPU and memory, but Kubernetes has support for other resource types as well, such as GPUs and more

Ex. `kuard-pod-resreq.yaml`

Resources are requested per container, not per Pod. The total resources requested by the Pod is the sum of all resources requested by all containers in the Pod. The reason for this is that in many cases the different containers have very different CPU requirements. For example, in the web server and data synchronizer Pod, the web server is user-facing and likely needs a great deal of CPU, while the data synchronizer can make do with very little

### Request Limit Details

Requests are used when scheduling Pods to nodes. The Kubernetes scheduler will ensure that the sum of all requests of all Pods on a node does not exceed the capacity of the node. Therefore, a Pod is guaranteed to have at least the requested resources when running on the node. Importantly, “request” specifies a minimum. It does not specify a maximum cap on the resources a Pod may use

Memory requests are handled similarly to CPU, but there is an important difference. If a container is over its memory request, the OS can’t just remove memory from the process, because it’s been allocated. Consequently, when the system runs out of memory, the kubelet terminates containers whose memory usage is greater than their requested memory. These containers are automatically restarted, but with less available memory on the machine for the container to consume

### Capping Resource Usage with Limits

Ex. `kuard-pod-reslim.yaml`

## Persisting Data with Volumes

Filesystems are deleted when a Pod is deleted or a container restarts

Sometimes we need persistent disk storage

### Using Volumes with Pods

`spec.volumes` section defines volumes that may be accessed by containers in the Pod manifest

Not all containers are required to mount all volumes defined in the Pod

`volumeMounts` defines volumes that are mounted into a particular container, and the path where each volume should be mounted. Two different containers in a Pod can mount the same volume at different mount paths

Ex. `kuard-pod-vol.yaml`

### Different Ways of Using Volumes with Pods

Communication - Ex. site & Git container

Cache - Ex. perf

Persistent Data - need for data that independent of the lifespan of a particular Pod

Mounting the Host Filesystem

### Persisting Data Using Remote Disks

Oftentimes, you want the data a Pod is using to stay with the Pod, even if it is restarted on a different host machine

To achieve this, you can mount a remote network storage volume into your Pod. When using network-based storage, Kubernetes automatically mounts and unmounts the appropriate storage whenever a Pod using that volume is scheduled onto a particular machine

There are numerous methods for mounting volumes over the network. Kubernetes includes support for standard protocols such as NFS and iSCSI as well as cloud provider–based storage APIs for the major cloud providers (both public and private). In many cases, the cloud providers will also create the disk for you if it doesn’t already exist

## Putting It All Together

Ex. `kuard-pod-full.yaml`

## Summary

Pods represent the atomic unit of work in a Kubernetes cluster. Pods are comprised of one or more containers working together symbiotically. To create a Pod, you write a Pod manifest and submit it to the Kubernetes API server by using the command-line tool or (less frequently) by making HTTP and JSON calls to the server directly

Once you’ve submitted the manifest to the API server, the Kubernetes scheduler finds a machine where the Pod can fit and schedules the Pod to that machine. Once scheduled, the kubelet daemon on that machine is responsible for creating the containers that correspond to the Pod, as well as performing any health checks defined in the Pod manifest

Once a Pod is scheduled to a node, no rescheduling occurs if that node fails. Additionally, to create multiple replicas of the same Pod you have to create and name them manually. In a later chapter we introduce the ReplicaSet object and show how you can automate the creation of multiple identical Pods and ensure that they are recreated in the event of a node machine failure