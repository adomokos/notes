# Kubernetes

Kubernetes (K8s) is open-source system for automating deployment, scaling and management of containerized applications.

### Features:
* Service Discovery/Load Balancing
* Storage Orchestration
* Automate Rollouts/Rollbacks
* Self-healing
* Secret and Configuration Management
* Horizontal Scaling

### Key Kubernetes Benefits

* Orchestrate Containers
* Zero-Downtime Deployments
* Self Healing (superpowers)
* Scale Containers

### Developer Use Cases

* Emulate production locally
* Move from Docker Compose to Kubernetes
* Create an end-to-end testing env
* Ensure application scales properly
* Ensure secrets/config are working properly
* Performance testing scenarios
* Workload scenarios (CI/CD and more)
* Learn how to leverage deployment options
* Help DevOps create resources and solve problems

### kubectl Command Line tool

`kubectl version` - check version
`kubectl cluster-info`
`kubectl get-all` - retrieve information about Kubernetes pods, deployments
`kubectl run [container-name] --image=[image-name]` - simple way to create a Deployment for Pod
`kubectl port-forward [pod] [ports]` - forward a port to allow external access
`kubectl expose ...` - expose a port for Deployment/Pod
`kubectl create [resource]` - create a resource
`kubectl apply [resource]` - create or modify a resource

### The Pod

A Pod is the basic execution unit of a Kubernetes application - the smallest and simplest unit in the Kubernetes object model that you create or deploy.

The POD has:
* IP
* Memory
* Volumes, etc - shared across containers

Can scale horizontally by adding Pod replicas.
Pods live and die, but never come back to life.

Pod containers share the same network namespace.
Pod containers have the same loopback network interface (localhost)
Ports can be reused ports in different pods.
Pods do not span nodes.

### Creating a Pod

`kubectl run [podname] --image=nginx:alpine` - Run the nginx:alpine container in a pod
`kubectl port-forward [name-of-pod] 8080:80` - Enable Pod container to be called externally

`kubectl delete pod [name-of-pod]` - will cause pod to be recreated
`kubectl delete deployment [name-of-deployment]` - will delete deployment that manages the pod

### Use YAML for describing pod

`kubectl create -f file.pod.yml` - will give an error if the pod already exists
`kubectl describe pod [pod-name]` - get more info about the pod
`kubectl apply i-f file.pod.yml -- save-config` - "--save-config" will store current properties in resource's annotations
`kubectl exec [pod-name] -it sh` - hop on the pod
`kubectl edit -f file.pod.yml` - edit the pod setting
`kubectl delete -f file.pod.yml` - delete the Pod using YAML file that created it

### Pod Health

Kubernetes relies on Probes to determine the health of a Pod container. A Probe is a diagnostic performed periodically by the kubelet on a container.

Types of Probes:
* Liveness Probe - helps determine if a Pod is healthy and running as expected
* Readiness Probe - heps determine if a Pod should receive requests

(Failed Pod containers are recreated by default - restartPolicy defaults to Always).

* ExecAction - executes an actions inside the container
* TCPSocketAction - TCP check against the container's IP address on a specified port
* HTTPGetAction - HTTP GET request against container

Probes can have the following results:
* Success
* Failure
* Unknown

### Deployment Core Concepts

A ReplicaSet is as declarative way to manage Pods. A Deployment is a declarative way to manage Pods using a ReplicaSet.
A ReplicaSet acts as a Pod controller:
* Self-healing mechanism
* Ensure the requested number of Pods are available
* Provide fault-tolerance
* Can be used to scale pods
* Relies on a Pod template
* No need to create Pods directly!
* Used by deployments

Deployments -> ReplicaSet -> Pod -> Container

Deployments manages Pods:
* Pods are managed using ReplicaSets
* Scales ReplicaSets which scale Pods
* Supports zero-downtime updates by creating and destroying RelicaSets
* Provides rollback functionality
* Creates a unique lable that is assigned to the ReplicaSet and generated Pods
* YAML is very similar to a ReplicaSet

## Creatign Deployments

`kubectl scale deployment [deployment-name] --replicas=5` - Scale the deployment to 5 Pods
`kubectl scale -f file.deployment.yml --replicas=5`
You can specify this in the YAML as well.

Demo:

```yaml
kubectl create -f nginx.deployment.yml --save-config
kubectl describe [pod | deployment] [pod-name | deployment-name]
kubectl apply -f nginx.pod.yml
kubectl get deployments --show-labels
kubectl get deployments -l app=my-nginx
kubectl scale -f nginx.deployment.yml --replicas=4
```

### Deployment Options

Zero downtime deployments allow software updates to be deployed to production without impacting end users.

* One of the strength of Kubernetes is zero downtime deployments
* Update an application's Pods without impacting end users
* Several options are available:
  - Rolling updates
  - Blue-green deployments
  - Canary deployments
  - Rollbacks

## Creating Services

Pods are "mortal" and may only live a short time (ephemeral).
You can't rely on a Pod IP address staying the same.
Pods can be horizontally scaled so each Pod get its own IP address.
A Pod gets an IP address after it has been scheduled (no way for clients to know IP ahead of time).

### The role of Services

* The goal of services is to abstract Pod IP addresses from consumers. Labels are important, as they are used to associate pods with a service.
* Load balances between the Pods
* Relies on labels to associate a Service with a Pod
* Node's kube-proxy creates a virtual IP for Services
* Layer 5 (TCP/UDP over IP)
* Services are not ephemeral
* Creates endpoints which sit between a Service and Pod

### Service Types

1. Default type - Cluster IP - expose the service on a cluster-internal IP
2. NodePort - Expose the service on each Node's IP at a static port.
3. LoadBalancer - Provision an external IP to act as a load balancer for the service
4. External Name - Maps a service to a DNS name

## Storage Options

A Volume can be used to hold data and state for Pods and containers.
Pods live and die soe ther file system is short lived (ephemeral).
Volumes can be used to store state/data and use it in a Pod.
A Pod can have multiple Volumes attached to it.
Containers rely on a mountPath to access a Volume.

### Volumes
A Volue references a storage location.
Must have a unique name.
Attached to a Pod and may or may not be tied to the Pod's lifetime (depending on the Volume type).
A Volume Mount references a Volume by name and defined a mountPath.

Volume Type Examples:
* emptyDir - Empty directory for storing "transient" data (shares of Pod's lifetime), useful for sharing files between containers running in a Pod.
* hostPath - Pod Mounts into the node's filesystem.
* nfs - A Network File System share mounts into the Pod
* configMap/secret - special types of volumes that provide a Pod with access to Kubernetes resources
* persistenVolumeClaim - provides Pods with a more persistent storage option that is abstracted from the details
* Cloud - Cluster-wide storage

### Persistent Volumes and Persistent Volume Claims

A PersistentVolume (PV) is a cluster-wide storage unit provisioned by an administrator with a lifecycle independent from a Pod.
A PersistentVolumeClaim (PVC) is a request for a storage unit (PV).

* A PersistentVolume is a cluster-wide storage resource that relies on network-atteched storage (NAS).
* Normally provisioned by a cluster administrator.
* Available to a Pod even if it gets rescheduled to a different Node.
* Rely on a storage provider such as NFS, cloud storage, or other options.
* Associated with a Pod by using a PersistentVolumeClaim (PVC).

### StorageClass

A StorageClass (SC) is a type of storage template that can be used to dynamically provision storage.

Used to define different "classes" of storage.
Act as a type of storage template.
Supports dynamic provisioning of PersistentVolumes.
Administrator don't have to create PVs in advance.

### Kubernetes StatefulSet:

Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

## ConfigMaps

ConfigMaps provide a way to store configuration information and provide it to containers.

* Provides a way to inject configuration data into a container
* Can store entire files or provide key/value pairs
  - file is the key value is the file content
  - provide on command-line
  - ConfigMap manifest

### Accessing ConfigMap Data in a Pod

ConfigMaps can be access from a Pod using:
* Environment variables (key/value)
* ConfigMap Volume (access as files)

### Creating a ConfigMap

* They can go into the `data` section of the YAML.
* They can be defined in a config file - `kubectl create configmap [cm-name] --from-file=[path-to-file]`
* Create an ENV file - `kubectl create configmap [cm-name] --from-env-file=[path-to-file]` (Note that the file name is *not* included as a key.)
* Use it literal - `kubectl create configmap [cm-name] --from-literal=apiUrl=https://my-api --from-literal=otherKey=otherValue`

Get a ConfigMap:
`kubectl get cm [cm-name] -o yaml`

## Sercrets

A Secret is an object that contains a small amount of sensitive data such as a password, a token or a key.

* Kubernetes can store sensitive information (passwords, keys, ceritificates, etc.)
* Avoids storing serets in container images, in files, or in deployment manifests.
* Mount secrets into pods as files or as environment variables.
* Kubernetes only makes secrets available to Nodes that have a Pod requesting the secrets.
* Secrets are stored in tmpfs on a Node (not on disk)

Secrets can be encrypted at rest.
Limit access to etcd (where Secrets are stored) to only admin users
Use SSL/TLS for etcd peer-to-peer communication.
Manifest (YAML/JSON) files only base64 encode the Secret
Pods Can access Secrets so secure which users can create Pods. Role-based access control (RBAC) can be used.

### Creating Secrets

`kubectl create secret generic my-secret --from-literal=pwd=my-password` - Create secret and store securely in Kubernetes
`kubectl create secret generic my-secret --from-file=ssh-privatekey=~/.ssh/id_rsa` - Create a secret from file
`kubectl create secret tls tls-secret --cert=path/to/tls.cert --key=path/to/tls.key` - Create a secret from a key pair

### Get secrets

`kubectl get secrets` - Get the secrets
`kubectl get secrets db-password -o yaml` - listing secrets

## HELM

Helm is a package manager for Kubernetes.

Charts has the various YAML files for Kubeneretes. Helm in the command line tool to interact with Tiller via gRPC. Tiller is the service that runs the generated yaml files for kubectl, will create the cluster.

Check helm's version:

```
helm version --short
Client: v2.16.1+gbbdfe5e
Server: v2.16.1+gbbdfe5e
```

Notice how the client and server has the same version, this is how it should be.

### Cleaning up

`helm list` will show the installations

`helm delete <name-of-install>` will delete everything related to that install

There are still some configmaps remaining items, find them with:

`kubectl get configmaps --namespace=kube-system`.

We can delete those with `helm delete <name-of-config-maps> --purge`

### Helm commands

`helm install [chart]` - Install a release
`helm upgrade [release] [chart]` - Upgrade a release revision
`helm rollback [release] [revision]` - Rollback to a release revision
`helm history [release]` - Print release history
`helm status [release]` - Display release status
`helm get [release]` - Show details of a release
`helm delete [release]` - Uninstall a release
`helm list` - List releases
