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
