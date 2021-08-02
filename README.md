# Kubernetes Handson

by Richard Chesterwood

## Introduction

For getting started with Kubernetes on Mac there is a good introduction at Romin Irani’s Blog [Getting Started with Kubernetes with Docker on Mac](https://rominirani.com/tutorial-getting-started-with-kubernetes-with-docker-on-mac-7f58467203fd)

This introduction is based upon a Udemy Course by Richard Chesterwood called [Kubernetes hands-on - deploy Microservices to the AWS Cloud](https://www.udemy.com/course/kubernetes-microservices)

## Welcome to Kubernetes

### Introducing Kubernetes

Kubernetes is an open-source _orchestration system_ for deploying, managing and scaling containerized applications.

The name Kubernetes originates from Greek, meaning helmsman (_german: Steuermann_) or pilot. Google open-sourced the Kubernetes project in 2014.

- This course is concentrating on Kubernetes, which is just one example of a container _orchestration system_.

- Kubernetes is commonly abbreviated to K8S, with eight just being the number of letters that are skipped.

- Containers have become a very popular way of deploying systems in the last few years, and by far the most popular containerization system is Docker.

If you're going deploy a microservice architecture, your next problem is how to manage a lot of Docker containers. What if a container crashes?

Even whenn you have only six containers, the you still need some kind of automatic system that helps to manage it and restarts your containers.

>**Note:** as Wikipedia page here says - Orchestration is the arrangement, coordination, and management of computer systems, middleware, and services.

### Why you need Kubernetes and what it can do

[Why you need Kubernetes and what it can do?](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/#why-you-need-kubernetes-and-what-can-it-do) Containers are a good way to bundle and run your applications. In a production environment, you need to manage the containers that run the applications and ensure that there is no downtime. For example, if a container goes down, another container needs to start. Wouldn't it be easier if this behavior was handled by a system?

That's how Kubernetes comes to the rescue! Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more. For example, Kubernetes can easily manage a canary deployment for your system.

Kubernetes provides you with:

- **Service discovery and load balancing** Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- **Storage orchestration** Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- **Automated rollouts and rollbacks** You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- **Automatic bin packing** You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- **Self-healing Kubernetes** restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- **Secret and configuration management** Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.

#### Manifests

An _orchestration system_ defines a series of simple text files called _manifests_.

- These manifests allow us to describe our system's entire architecture
- They allow us to describe what containers we want to run
- They allow us to describe what infrastructure we want to run, so just hard discs or nodes

Even if there's some kind of catastrophe, which means containers have crashed, then the orchestration system will be responsible for restoring the system back to it's healthy state described in the manifests.

#### Kubernetes History

Kubernetes has originated from Google and has presumably been used to manage their multi-billion container deployments.

Kubernetes is now actually independent of Google is managed by an organisation called the _Cloud Native Computing Federation_. This is a project that has over 200 different organisations contributing to it.

## Install Kubernetes for local Development

Install Docker on your mac and activate kubernetes.

```bash
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.3",GitCommit:"2bba0127d85d5a46ab4b778548be28623b32d0b0", GitTreeState:"clean", BuildDate:"2018-05-21T09:17:39Z", GoVersion:"go1.9.3", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.3", GitCommit:"2bba0127d85d5a46ab4b778548be28623b32d0b0", GitTreeState:"clean", BuildDate:"2018-05-21T09:05:37Z", GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}
```

```yaml
$ kubectl config current-context
docker-for-desktop
```

```bash
kubectl get pods — namespace=kube-system
```

## Docker Quickstart

### Docker Overview

Docker enables us to create so-called _containers_ which are self-contained environments containing a complete Linux distribution and any application that you want to deploy.

The idea is that because these containers are complete, you can take a container and deploy it straight to a server or your own desktop computer or any computer and that container will run in exactly the same way as long as Docker is installed.

### Docker Containers vs. Images

There are two concepts containers and images.

- An image is a definition of a container
- An image is a binary file that contains all of the software and things like environment variables, and general settings that the container will need to run

If you want to run a webapp then you have to install all the things needed on you local machine or server,like Java, MySQL, Python, NGINX.

- The difference with an image is, the developer can package everything that is needed, every single dependency required, to make that application run into the image
- The idea is to pass the image to the deployer, and he only has to run the image
- When we run an image, then we call it a _container_

#### Publish images

At the time of recording, the most popular place to publish images is this site here called `hub.docker.com`.

### Running Docker Containers

#### Basic Docker commands

Let me show you one of the most common Docker commands, which is  `docker image ls`.

That is saying, "Give me a list of all of the images "that I have downloaded to this computer."

```bash
% docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mongo               latest              aa22d67221a0        1 day ago        493MB
node                12-alpine           18f4bc975732        1 day ago        89.3MB
nginx               alpine              ecd67fe340f9        1 day ago        21.6MB
```

What's happening here is that the Docker command line, this is where you were entering that docker command, is trying to connect to Docker itself, which, typically, you run as a separate daemon process.

#### Running the Docker Daemon inside Minikube

At the moment of recording, inside of Minikube there is running a Docker Daemon. If we want to use it, then we have to set up some environment variables.

```bash
eval $(minikube docker-env)
```

### Running Containers from DockerHub

You can find millions of conatiners at DockerHub. One of them is [richardchesterwood/k8s-fleetman-webapp-angular](https://hub.docker.com/r/richardchesterwood/k8s-fleetman-webapp-angular).


<div class="page"/>

## Kubernetes Pods

A Pod is the _Basic Execution Unit of a Kubernetes application_. It is the smallest and simplest unit in the Kubernetes object model that you create or deploy.

## Pods Overview

You can think of a pod as a wrapper for a container and most of the time you can think of a pod and a container to be in a one-to-one relationship.

It is possible to have more than one container in a pod. There might be some kind of helper container, e.g. MongoDB uses them and they call them side-car containers. What you'd definitley would never do is, have two microservices inside one pod.

Understanding Pods

- A Pod represents processes running on your Cluster - see <https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/>.
- A Pod encapsulates an application’s container (or, in some cases, multiple containers), storage resources, a unique network IP, and options that govern how the container(s) should run.
- A Pod represents a unit of deployment: a single instance of an application in Kubernetes, which might consist of either a single container or a small number of containers that are tightly coupled and that share resources.

### Writing a pod

You have to create a _yaml file_ for your pod, e.g. `first-pod.yaml`. Please take care that you don't use tabulators in yaml files!

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
  labels:
    app: webapp
spec:
  containers:
  - name: webapp
    image: richardchesterwood/k8s-fleetman-webapp-angular:release0
```

The yaml file consists of the following parts.

- We just need a line at the top that says `apiVersion: v1`
- We're defining an object in kubernetes of `kind: Pod`
- The `metadata` block allows us to give the pod a name `name: webapp`, in fact we have to give the pod a name
- Pods can have labels which are key/value pairs like `app: webapp`
- A selector is a series of key/value pairs
- A service is looking for a pod with a matching selector
- As part of that spec, we list the containers that we want in the pod
- And remember, generally, we're gonna be going for one container per pod.
- So for each container, we will give the container a name.
- We need to specify which image that container is going to be built from
- And then, optionally, we can supply command and arguments

In practice you don't create Pods, you create a _Replicaset_ or a _Deployment_.

> **_Warning_**
It is recommended that users create Pods only through a Controller, and not directly. A Controller is a _Deployment_, _Job_, or _StatefulSet_.

For more information look at the [API overview Pod-V1-Core](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/#pod-v1-core) Chapter in the reference.

### Running a pod

Make sure you are in the same directory as your yaml file.

```bash
$ ls
first-pod.yaml
```

Most important command is `kubectl`. You can show everything we have created using `kubectl get all`.

```bash
$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   67d
```

There is one service running which is _service/kubernetes_. This is a rest service. Every time we run _kubectl_, _kubectls_ is posting commands to this rest service api endpoint.

We need to tell kubernetes that we want to deploy a pod to the cluster. And we do that with the following command.

```bash
$ kubectl apply -f first-pod.yaml
pod "webapp" created
```

What you get is.

```bash
$ kubectl get all
NAME         READY     STATUS    RESTARTS   AGE
pod/webapp   1/1       Running   0          22s
```

Remember the following Note.

>_Note:_ Pods are not visible outside the cluster!

So you can't access them outside the cluster!

### Delete a Pod

To delete the old version of the pod you can do an `kubectl delete pod <name of the pod>`.

```bash
kubectl delete pod webapp
```

Or if you want to delete all pods.

```bash
kubectl delete pods --all
```

Or maybe only the pods defined in a certain file.

```bash
kubectl delete pod -f pods.yaml
```

### Describe a Pod

To find out more information on a pod, we can use `kubectl describe pod <podname>`.

```bash
$ kubectl describe pod webapp
Name:         webapp
Namespace:    default
Node:         docker-for-desktop/192.168.65.3
Start Time:   Tue, 17 Dec 2019 15:22:05 +0100
Labels:       app=webapp
              release=0
Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"webapp","release":"0"},"name":"webapp","namespace":"default"},"spec":{"co...
Status:       Running
IP:           10.1.0.99
Containers:
  webapp:
    Container ID:   docker://2f896b1064d5b54819faeb9b0de7e9ce114efd1028047c842f4168657ffff961
    Image:          richardchesterwood/k8s-fleetman-webapp-angular:release0
    Image ID:       docker-pullable://richardchesterwood/k8s-fleetman-webapp-angular@sha256:9b98fec20772bd1d7d4c9085048f28af35b31ad3a7b7d3ba395fb512c5c359e6
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 17 Dec 2019 15:22:07 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-5d89q (ro)
Conditions:
  Type           Status
  Initialized    True
  Ready          True
  PodScheduled   True
Volumes:
  default-token-5d89q:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-5d89q
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:          <none>
```

### Access a Pod

Run a command in the container using `kubectl exec <podname> <command to execute>`.

```bash
kubectl exec webapp ls
```

Get a shell in this container whereby _-it_ means to get in interactively with teletype emulation.

```bash
kubectl -it exec webapp sh
```

This way is deprecated, it is better to use the following command.

```bash
kubectl exec --stdin --tty mongodb-5b4859859c-6l7kn -- /bin/bash
```

Get a url or better get localhost:

```bash
printf "GET /\n\n" | nc google.com 80
printf "GET /\n\n" | nc localhost 80
```

Or use _wget_.

```bash
wget http://localhost:80
cat index.html
```

Exit the shell with _exit_.

### Additional information on Pods

Show labels...

```bash
$ kubectl get po --show-labels
NAME                READY     STATUS    RESTARTS   AGE       LABELS
webapp              1/1       Running   2          3h        app=webapp,release=0
webapp-release0-5   1/1       Running   0          7m        app=webapp,release=0-5
```

<https://kubernetes.io/docs/reference/>

```bash
$ kubectl cluster-info
Kubernetes master is running at https://localhost:6443
KubeDNS is running at https://localhost:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

```bash
$ kubectl get nodes
NAME                 STATUS    ROLES     AGE       VERSION
docker-for-desktop   Ready     master    67d       v1.10.3
```


<div class="page"/>

## Services

Pods are designed to be very _throw away things_. They have short lifetimes, they regularly die, and they are regularly recreated.
As I mentioned in the introduction section, we treat Pods like cattle, we don't treat them like pets, since we can scale them up.

Kubernetes has a further concept called a service.

- Services are long-running objects.
- They have an ip-address and a stable fixed port
- We can attach services to pods

With a service we can connect to the Kubernetes Cluster and the service will find a suitable Pod to service that request.


The next important concept that we haven't met yet is the concept of a _Pod Label_. What we can do with a Pod is we can set up a series of _key value pairs_, for example we might give a Pod a label whose _name_ is _app_ and the _value_ is _webapp_.

We connect services to Pods using _Pod Labels_. When we write our service we provide a so-called selector and the selector is itself a series of _key value pairs_.

Now what happens at run time in the Kubernetes Cluster is that the service will look for any matching key value pairs amongst the running Pods and where it finds a matching key value pair it will connect to that Pod.

### Get services running

```bash
$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4h
```

```bash
kubectl describe pod webapp
```

### Write a service

Services are network endpoints for other services or maybe external users that want to connect to it, e.g. a browser.

Put the following definition into a file _services.yaml_.

```yaml
apiVersion: v1
kind: Service
metadata:
  # Unique key of the Service instance
  name: fleetman-webapp
spec:
  selector:
    # Loadbalance traffic across Pods matching
    # This defines what services are represented by this service
    # The service is a network endpoint for other services
    # or maybe external users that want to connect to, e.g. browser
    app: webapp

  ports:
    # Accept traffic sent to port 80
    - name: http
      port: 80
      targetPort: 80
      nodePort: 30080

  type: NodePort
```

In order to map a port from inside the container/pod to the kubernetes cluster, we define the _port_ which is mapped to the _targetPort_ in the container. We can compare this to the `-p` docker parameter, e.g. `-p 8080:8080`.

In order to map the port there are three _Types_ that are available.

- _ClusterIP_ means that the ip is only accessible inside the kubernetes cluster to other nodes and a nodePort is not allowed! This makes it a kind of private service. It will give this service an ip address.
- _NodePort_ this is the way how we are going to expose a port through the node, the node in our case is the entire Kubernetes Cluster and this is how we can choose what port we're going to be exposing to the outside world
- _Loadbalancer_

Services have _ports_ that are mapped to an _internal port_ called `targetPort` and an _external port_ called `nodePort`. 

> Note: take care that you have to set the `tagetPort` in a Spring Boot to `8080`.

Restrictions on Nodeports:

- The _nodePort: 30080_ exposes the port _80_ to the outside world and maps it to port _30080_
- A _nodePort_ must be greater than _30000_

The apparent reasoning for that restriction is just that, the Kubernetes designers want to avoid the possibility of having any clashes of ports, since unless we specify otherwise, all of these port numbers are automatically generated by Kubernetes and therefore Kubernetes can make sure that there are no clashes of port numbers.

When we move on to a real cloud environment, then we're going to be replacing this NodePort with a very featureful loadbalancer, and that loadbalancer will be easily able to expose a sensible port like port 80.

### Pod Selection With Labels

Updating a service with no downtime is easy and we can achieve this by using different selectors/labels.

The easiest way to get all pods is using the `kubectl get pods` command.

```bash
kubectl get pods
```

You can switch seamlessly between two versions of an app, if you start the new version of the pod whereby the selector changes, and afterwards point the service to the new pod. Here we change the release label from `release: "0"` to `release: "0-5"`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
  labels:
    app: webapp
    release: "0"
spec:
  containers:
  - name: webapp
    image: richardchesterwood/k8s-fleetman-webapp-angular:release0
---
apiVersion: v1
kind: Pod
metadata:
  name: webapp-release-0-5
  labels:
    app: webapp
    release: "0-5"
spec:
  containers:
  - name: webapp
    image: richardchesterwood/k8s-fleetman-webapp-angular:release0-5
```

Now let's apply these changes.

```bash
kubectl apply -f pods.yaml
```

If you add the `--show-labels` option, then you can list all labels of the pods.

```bash
$ kubectl get pods --show-labels
NAME                READY     STATUS              RESTARTS   AGE       LABELS
queue               0/1       ContainerCreating   0          17s       app=queue,release=1
webapp              1/1       Running             2          3h        app=webapp,release=0
webapp-release0-5   1/1       Running             0          1h        app=webapp,release=0-5
```

And you can list only the pods with a specific label.

```bash
kubectl get pods --showlabels -l release=0
```

Now you can switch the pod and the version with no downtime by changing the _selector_ in the service from `release: "0"` to `release: "0-5"`.


<div class="page"/>

## ReplicaSets

A ReplicaSet’s purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.

A ReplicaSet ensures that a specified number of _pod replicas_ are running at any given time. However, a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features. Therefore, we recommend using Deployments instead of directly using ReplicaSets, unless you require custom update orchestration or don’t require updates at all.
This actually means that you may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.

### Write a ReplicaSet

The only slight difference is, and it's a bit subtle, this, all of these different API structures are put into kind of folders. They call them groups. 

For the API version, we just had to say v1, because this was from the `core` group. But when we use ReplicaSets, because they're part of a different group, they're part of `apps`,

Create a new yaml file called `webapp-replicaset.yaml`. This ReplicaSet yaml file is sort of a two in one. It's a combination of a pod definition and a ReplicaSet definition..

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: webapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: richardchesterwood/k8s-fleetman-webapp-angular:release0-5
```

In the Replicaset, we have an outermost tag called spec, which has a `replicas` key and a `template` key. The template contains a `metadata` and a `spec` key that is exactly what we previously had in the pod definition. It's just that instead of being sort of at the top level, it's now a nested element.

But what is a little bit weird, we additionally need a selector with some `matchLabels`in it. Otherwise we get an error.

Saving this manifest into  `webapp-replicaset.yaml` and submitting it to a Kubernetes cluster will create the defined ReplicaSet and the Pods that it manages.

```bash
kubectl apply -f  webapp-replicaset.yaml
```

You can then get the current ReplicaSets deployed:

```bash
kubectl get rs
```

### Describe a ReplicaSet

To get more information about a Replicaset you simply use `kubectl describe replicaset <NAME>`.

```bash
$ kubectl describe replicaset webapp
Name:         webapp
Namespace:    default
Selector:     app=webapp
Labels:       app=webapp
Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"ReplicaSet","metadata":{"annotations":{},"name":"webapp","namespace":"default"},"spec":{"replicas":1,"selector":{"match...
Replicas:     1 current / 1 desired
Pods Status:  1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=webapp
  Containers:
   webapp:
    Image:        richardchesterwood/k8s-fleetman-webapp-angular:release0-5
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  5m    replicaset-controller  Created pod: webapp-rz9jg
```

Or short...

```bash
kubectl describe rs webapp
```

## Delete a ReplicaSet

You can delete a _replicaset_ (_rs_) by using the following command:

```bash
kubectl delete rs webapp
```

This will terminate the _pods_ that were running.

## Deployments

Deployments are more sophisticated replica sets.

- With deployments we get rolling updates with zero downtime
- The structure of the yaml file is the same as of a replicaset

The difference is that there are some additional field for rolling updates.

### Write a Deployment

We can pretty much literally just change `kind: ReplicaSet` to `kind: Deployment` and this is now become a deployment.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: richardchesterwood/k8s-fleetman-webapp-angular:release0-5
```

You can think of a deployment as being an entity in Kubernetes that manages the replica set for you. A _deployment_ will create a corresponding _replicaset_, which will create the pods.

```bash
$ kubectl get all
NAME                         READY     STATUS    RESTARTS   AGE
pod/queue                    1/1       Running   0          2d
pod/webapp-ccb5c74c9-jkkh9   1/1       Running   0          6s

NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/fleetman-queue    NodePort    10.101.105.73   <none>        8161:30010/TCP   2d
service/fleetman-webapp   NodePort    10.97.57.139    <none>        80:30080/TCP     2d
service/kubernetes        ClusterIP   10.96.0.1       <none>        443/TCP          70d

NAME                     DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/webapp   1         1         1            1           6s

NAME                               DESIRED   CURRENT   READY     AGE
replicaset.apps/webapp-ccb5c74c9   1         1         1         6s
```

To get a rolling update with zero downtime, we change the version of the image to a new version and can add _minReadySeconds_. The field _minReadySeconds_ - Minimum number of seconds defines how many seconds a newly created pod should be ready for it to be considered available.

### Managing rollouts

Show the status of a pending rollout:

```bash
kubectl rollout status deployment webapp
```

Show the history of a deployment:

```bash
kubectl rollout history deployment $ kubectl rollout history deployment webapp
deployments "webapp"
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
```

Undo the last rollout if something went horribly wrong:

```bash
kubectl rollout undo deployment webapp
```

Or with a specific revision number:

```bash
kubectl rollout undo deployment webapp --to-revision=2
```

<div class="page"/>

## Networking and Service Discovery

In this section we will find out how _Networking_ and _Service Discovery_ in kubernetes works.
### Networking overview

Assume we want an application in one _container_ communicate with an _database_ in another _container_. We could put them in a single pod and they could talk to each other using `localhost` but that would be no good idea because we could not scale our application. This is why we put them into separate pods and expose the functionality as services. Each service will have its own _ip address_. If the application wants to communicate with the database it will use the _ip address_ of the _database service_. Since this _ip address_ won't be fixed we will need a _DNS service_.

Kubernetes maintains its own DNS service _kube-dns_. A DNS service contains a set of key value pairs that maps dns names to ip-addresses.

### Namespaces

If you search for the _kube-dns_ pod by using `kubectl get all` you won't find it, because it belongs to a different a namespace and `kubectl get all` only shows the pods with namespace `default`.

A namespace is a way to partition your resources into separate areas.
Get all namespaces:

```bash
$ kubectl get ns
NAME          STATUS    AGE
default       Active    70d
docker        Active    70d
kube-public   Active    70d
kube-system   Active    70d
```

Get all pods in namespace _kube-system_ where you will find the _kube_dns_:

```bash
$ kubectl get po -n kube-system
NAME                                         READY     STATUS    RESTARTS   AGE
etcd-docker-for-desktop                      1/1       Running   0          70d
kube-apiserver-docker-for-desktop            1/1       Running   0          70d
kube-controller-manager-docker-for-desktop   1/1       Running   0          70d
kube-dns-86f4d74b45-llqx8                    3/3       Running   0          70d
kube-proxy-l7t5w                             1/1       Running   0          70d
kube-scheduler-docker-for-desktop            1/1       Running   0          70d
kubernetes-dashboard-669f9bbd46-ktlk9        1/1       Running   1          70d
```

Our _kube-dns_ pod has the name `kube-dns-86f4d74b45-llqx8`.
Get all information for a given namespace:

```bash
kubectl get all -n kube-system
```

And if we execute this command we get.

```bash
% kubectl get all -n kube-system 
NAME                                         READY   STATUS    RESTARTS   AGE
pod/coredns-5644d7b6d9-64mf7                 1/1     Running   0          26m
pod/coredns-5644d7b6d9-6p2wm                 1/1     Running   0          26m
pod/etcd-docker-desktop                      1/1     Running   0          26m
pod/kube-apiserver-docker-desktop            1/1     Running   0          24m
pod/kube-controller-manager-docker-desktop   1/1     Running   0          23m
pod/kube-proxy-6c775                         1/1     Running   0          26m
pod/kube-scheduler-docker-desktop            1/1     Running   0          23m
pod/storage-provisioner                      1/1     Running   0          23m
pod/vpnkit-controller                        1/1     Running   0          23m

NAME               TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
service/kube-dns   ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   27m

NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
daemonset.apps/kube-proxy   1         1         1       1            1           beta.kubernetes.io/os=linux   27m

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/coredns   2/2     2            2           27m

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/coredns-5644d7b6d9   2         2         2       26m
```

Which shows the _ip address_ of the _kubernetes dns_ `service/kube-dns` is `10.96.0.10`.

### Kubernetes DNS lookup

Kubernetes automatically configures the _dns system_ for all of its containers/pods. We can log in in one of our containers to show this.

```bash
$ kubectl exec -it webapp-746ff4c965-fnk22 sh
/ # cat /etc/resolv.conf
nameserver 10.96.0.10
search default.svc.cluster.local svc.cluster.local cluster.local
options ndots:5
```

We can verify this and show the `/etc/resolve.conf`  file.

```bash
# cat /etc/resolv.conf 
nameserver 10.96.0.10
search default.svc.cluster.local svc.cluster.local cluster.local
options ndots:5
```

Now we can lookup the _ip address_ using _nslookup_:

```bash
$ # nslookup database
Name:      database
Address 1: 10.96.72.203 database.default.svc.cluster.local
```

If we do a `kubectl get all` we should see if the _ip address_ is correct.

```bash
% kubectl get all
NAME                          READY   STATUS              RESTARTS   AGE
pod/mysql                     0/1     ContainerCreating   0          27s
pod/webapp-858957ccc9-nprrq   1/1     Running             0          5m27s
pod/webapp-858957ccc9-wnb8l   1/1     Running             0          5m27s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
service/database     ClusterIP   10.96.72.203   <none>        3306/TCP   27s
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP    40m

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/webapp   2/2     2            2           5m27s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/webapp-858957ccc9   2         2         2       5m27s
me@Simons-MBP Chapter 10 Networking % kubectl get all
NAME                          READY   STATUS    RESTARTS   AGE
pod/mysql                     1/1     Running   0          36s
pod/webapp-858957ccc9-nprrq   1/1     Running   0          5m36s
pod/webapp-858957ccc9-wnb8l   1/1     Running   0          5m36s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
service/database     ClusterIP   10.96.72.203   <none>        3306/TCP   36s
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP    40m

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/webapp   2/2     2            2           5m36s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/webapp-858957ccc9   2         2         2       5m36s
```

That means we could use the _service name_ wherever we want.

### Fully Qualified Domain Names (FQDN)

The _database_ is not registered under _database_ but under its _Fully Qualified Domain Name_.

```bash
$ # nslookup database
Name:      database
Address 1: 10.96.72.203 database.default.svc.cluster.local
```

In our case the _FQDN_ is `database.default.svc.cluster.local`. But `nslookup` won't find `database` and it searches by appending `.default.svc.cluster.local` which is configured in `/etc/resolv.conf`.

<div class="page"/>

## Microservice Architectures

### An Introduction to Microservies

A monolith...

- is a traditional web application
- is deployed as a single unit - e.g. a single war file
- is often fullfilling many business needs, e.g. a cart management, a catalogue management, a reporting system,..
- uses often one global database

The problem is that...

- monoliths often get too big to be manageble
- if there many teams, we have to coordinate new deployments
- talk to other teams to assure that my changes don't impact them

A Microservice...

- can function on its own
- can be deployed on its own
- can be developed on its own
- can communicate with other microservices through well defined interfaces
- should be dealing with only one specific business area - _single responsibility priciple_
- should be highly cohesive - should have a single responsibility
- should be loosely coupled

Single Responsibility Principle

- a class should have only one reason to change

### Overview of the system

So in the previous videos I've introduced you to the high-level
architecture of the system. You don't need to understand this system in any great detail, but I hope you at least understand the reasons for why we have five separate components for this system.

Just to re-emphasize, a real microservice architecture will have many, many, many more microservices. This is designed to be the simplest possible microservice architecture that could work.

We have five elements.

- a web front end which is implemented in java script, that's calling an API gateway
- a API gateway that connects the frontend to the backend
- the backend which is a microservice called the _position tracker_, that's calculating the speeds of vehicles and storing the positions of all the vehicles
- a queue which is going to store the messages, that are received from the vehicles as they move around the country
- a microservice called the position simulator, which is going to generate some positions of vehicles for testing our system

### Deploying the Queue

I think it would be sensible if we started from a nice clean sheet since we have some pods and services already running.

```bash
$ kubectl delete -f pods.yaml
deployment.apps "webapp" deleted
pod "queue" deleted
```

And we also delete our services.

```bash
kubectl delete -f services.yaml
```

Now we can start and deploy our queue. For this we rename our `pods.yaml` file to `workloads.yaml`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: queue
spec:
  selector:
    matchLabels:
      app: queue
  replicas: 1
  template: # template for the pods
    metadata:
      labels:
        app: queue
    spec:
      containers:
      - name: queue
        image: richardchesterwood/k8s-fleetman-queue:release1
```

We also need a corresponding service. And really the only complicated thing about the queue is knowing which ports we need to expose in the service.

So there are two ports that we need to work with. In Apache ActiveMQ the port `8161` is the admin console. We don't need this console, but it will be very useful when we're getting the system up and running to see this admin consol in action. We map this port to `nodePort: 30010` to access it from outside of the cluster.

There is another port which is far more important than the admin console, and that's port `61616`, and that's the port that we use to send and receive messages.

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: fleetman-queue

spec:
  # This defines which pods are going to be represented by this Service
  # The service becomes a network endpoint for either other services
  # or maybe external users to connect to (eg browser)
  selector:
    app: queue

  ports:
    - name: http
      port: 8161
      nodePort: 30010

    - name: endpoint
      port: 61616

  type: NodePort
```

Now we can apply these changes.

```bash
kubectl apply -f workloads.yaml
```

And we can start our service.

```bash
kubectl apply -f services.yaml
```

Now if we log into the _ActiveMQ console_ using `http://localhost:30010` and click on _Manage Active MQ broker_, then we can log in using username `admin` and password `admin`.

## Deploying the Position Simulator

And now onto what is really the first of the microservices to deploy. I think we're going to start with the _position simulator_.

There are no ports needed for this particular micro service,
it runs in a sort of isolated form. And we don't need a service definition for this in Kubernetes either.

The one complexity, and this is something we haven't covered so far, is that the developers told me that it does need an environment variable setting.

If you are interested in the Code you can find the [Posititon Simulator here](https://github.com/DickChesterwood/k8s-fleetman/tree/master/k8s-fleetman-position-simulator).

Depending on the environment variable set, the position tracker uses a different configuration file, e.g. `/src/main/resources/application-production-microservice.properties` when we set `SPRING_PROFILES_ACTIVE` to `production-microservice`.

The properties file then contains the url for the production system.

```java
spring.activemq.broker-url=tcp://fleetman-queue.default.svc.cluster.local:61616
fleetman.position.queue=positionQueue
```

Now we can add the _position simulator_ to our _workloads_ file.

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: position-simulator
spec:
  selector:
    matchLabels:
      app: position-simulator
  replicas: 1
  template: # template for the pods
    metadata:
      labels:
        app: position-simulator
    spec:
      containers:
      - name: position-simulator
        image: richardchesterwood/k8s-fleetman-position-simulator:release1
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: production-microservice
```

Now we can apply these changes.

```bash
kubectl apply -f workloads.yaml
```

### Inspecting Pod Logs

We can inspect the logs of a pod to find errors when starting a pod. For this we get the name of the pod. And then if we want to inspect the position simulater pod we can do a `kubectl logs`.

```yaml
kubectl logs position-simulator-ccb5c74c9-jkkh9
```

Now if we log into the _ActiveMQ console_ using `http://localhost:30010` you should see some pending messages.


### Deploying the Position Tracker

The _Position Tracker_ is the main backend that tracks all _Vehicle Positions_.

It offers a _REST Api_ for the _API Gateway_ which we will deploy later.

```java
@RestController
public class PositionReportsController {

  @Autowired
  private Data data;
  
  @RequestMapping(method=RequestMethod.GET,value="/vehicles/{vehicleName}")
  public ResponseEntity<VehiclePosition> getLatestReportForVehicle(@PathVariable String vehicleName) {
    try {
      VehiclePosition position = data.getLatestPositionFor(vehicleName);
      return new ResponseEntity<VehiclePosition>(position, HttpStatus.OK);
    } catch (VehicleNotFoundException e) {
      return new ResponseEntity<>(HttpStatus.NOT_FOUND);
    }
  }
  
  @RequestMapping(method=RequestMethod.GET, value="/history/{vehicleName}")
  public Collection<VehiclePosition> getEntireHistoryForVehicle(@PathVariable String vehicleName) throws VehicleNotFoundException {
    return this.data.getHistoryFor(vehicleName);
  }
  
  @RequestMapping(method=RequestMethod.GET, value="/vehicles/")
  public Collection<VehiclePosition> getUpdatedPositions(@RequestParam(value="since", required=false) @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) Date since) {
    Collection<VehiclePosition> results = data.getLatestPositionsOfAllVehiclesUpdatedSince(since);
    return results;
  }
}
```

So in the `workloads.yaml` file we add the following lines and we're going to call this `position-tracker`.

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: position-tracker
spec:
  selector:
    matchLabels:
      app: position-tracker
  replicas: 1
  template: # template for the pods
    metadata:
      labels:
        app: position-tracker
    spec:
      containers:
      - name: position-tracker
        image: richardchesterwood/k8s-fleetman-position-tracker:release1
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: production-microservice
```

And we add the following lines to our `services.yaml`.

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: fleetman-position-tracker

spec:
  # This defines which pods are going to be represented by this Service
  # The service becomes a network endpoint for either other services
  # or maybe external users to connect to (eg browser)
  selector:
    app: position-tracker

  ports:
    - name: http
      port: 8080

  type: ClusterIP
```

Now we can apply these changes.

```bash
kubectl apply -f workloads.yaml
kubectl apply -f services.yaml
```

### Deploying the API Gateway

And now we create the _API Gateway_, that contains our main Access Point for our Frontend. It contains a _VehicleController_.

```java
@Controller
@RequestMapping("/")
public class VehicleController {
  ...

  @GetMapping("/")
  @ResponseBody
  /**
  * This is just a test mapping so we can easily check the API gateway is standing.
  * When running through the Angular Front end, can visit this URL at /api/
  */
  public String apiTestUrl() {
    return "<p>Fleetman API Gateway at " + new Date() + "</p>";
  }

  @GetMapping("/history/{vehicleName}")
  @ResponseBody
  @CrossOrigin(origins = "*")
  public Collection<LatLong> getHistoryFor(@PathVariable("vehicleName") String vehicleName) {
    ...
    return results;
  }
  ...
}
```

We need to expose port 8080 to the cluster and if you want to test this, then go to the browser and visit `http://localhost:8080/`.

So in the `workloads.yaml` file we add the following lines and we're going to call this `api-gateway`.

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-gateway
spec:
  selector:
    matchLabels:
      app: api-gateway
  replicas: 1
  template: # template for the pods
    metadata:
      labels:
        app: api-gateway
    spec:
      containers:
      - name: api-gateway
        image: richardchesterwood/k8s-fleetman-api-gateway:release1
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: production-microservice
```

And we add the following lines to our `services.yaml`.

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: fleetman-api-gateway

spec:
  # This defines which pods are going to be represented by this Service
  # The service becomes a network endpoint for either other services
  # or maybe external users to connect to (eg browser)
  selector:
    app: api-gateway

  ports:
    - name: http
      port: 8080
      nodePort: 30020

  type: NodePort
```

Now we can apply these changes.

```bash
kubectl apply -f workloads.yaml
kubectl apply -f services.yaml
```

We can add a nodePort `nodePort: 30020` here temporarily to test this. Later we can remove this and change the type to `type: ClusterIP`.

Now we can apply our changes.

```bash
kubectl apply -f workloads.yaml
kubectl apply -f services.yaml
```

And we can test this in our browser using the minikube's ip address or `localhost:30020` in Docker Desktop.

### Deploying the Webapp

Now let's add our frontend to our `workloads.yaml`.

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  selector:
    matchLabels:
      app: webapp
  replicas: 1
  template: # template for the pods
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: richardchesterwood/k8s-fleetman-webapp-angular:release1
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: production-microservice
```

And we need to add a service to our `services.yaml`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: fleetman-webapp

spec:
  # This defines which pods are going to be represented by this Service
  # The service becomes a network endpoint for either other services
  # or maybe external users to connect to (eg browser)
  selector:
    app: webapp

  ports:
    - name: http
      port: 80
      nodePort: 30080

  type: NodePort
```

And we can test this in our browser using the minikube's ip address and port `30080` or `localhost:30080` in Docker Desktop.

![Fleetman Webapp](./k8s-fleetman-webapp.png)

<div class="page"/>

## Kubernetes Persistence and Volumes

### Upgrading to a Mongo Pod

First thing we have to do is to upgrade to the new release 3 of our _Position Tracker_ which uses a mongo database.

We use the latest official [_mongo image_](https://hub.docker.com/_mongo) that could be found at docker hub, which is in our case version `3.6.5-jessie`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  selector:
    matchLabels:
      app: mongodb
  replicas: 1
  template: # template for the pods
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:3.6.5-jessie
```

You can setup a mount-point for your mongodb if you want persistent storage...

```yaml
      volumeMounts:
          - name: mongo-persistent-storage
            mountPath: /data/db
    volumes:
      - name: mongo-persistent-storage
        hostpath:
          path: /mnt/some/directory/structure/
          type: DirectoryOrCreate
```

We should also create a service in order to connect to it.

```yaml
---
kind: Service
apiVersion: v1
metadata:
  name: fleetman-mongodb
spec:
  selector:
    app: mongodb
  ports:
    - name: mongoport
      port: 27017
  type: ClusterIP
```

### Volume mounts

### Persistent volume claims & Persistent Volumes

Persistent volume claim

- defines what we need
- look for a matching _persistent volume_ that fulfills its needs

Persistent volumes

- define how the storage is implemented, e.g. ssd-drive
- so it defines a physical storage

### Storage Classes & binding

_Persistent volume claims_ and _Persistent volumes_ are bound together using _Storage Classes_.
While binding them together kubernetes searches for a match using the _storageClassName_, _accessModes_, and the _storage_.

### Write a storage file

```yaml
# What do want?
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-pvc
spec:
  storageClassName: mylocalstorage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
---
# How do we want it implemented
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-storage
spec:
  storageClassName: mylocalstorage
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/some new/directory/structure/"
    type: DirectoryOrCreate
```

Get a list of all your persistent volumes and claims

```bash
kubectl get pv
kubectl get pvc
```

Now you can create the volumes

```bash
kubectl create -f local-volumes.yaml
```

<div class="page"/>

## Running Kubernetes on the AWS Cloud

Well it's finally time to get into the cloud. We will deploy our system to some real hardware. For the course, I'm going to use AWS. But as we've already mentioned, Kubernetes can run on any of the major could platforms, Google, Azure, and many, many more.

### Getting started with AWS

In fact, one of the key selling points for Kubernetes is that Kubernetes can automate much of your cloud platform. We're not really going to have to create any EC2 instances, or load balances or auto-scale groups because Kubernetes is going to do all of that for us.

When we're going to production on a cloud environment, we're going to create several nodes. A node is a physical server in our system.
As we're planning to deploy onto Amazon Web Services, a node in AWS is called an _EC2 instance_.

We're typically going to only deploy a small proportion of our system to a single node. Typically in Kubernetes we're going to have
multiple nodes in a so-called cluster.

Now one of the great joys of working in Kubernetes is we don't have to make much in the way of decisions to which nodes these pods are deployed to, because we're going to have a further node called the master node, and the master node is responsible for scheduling these pods. And scheduling here is just jargon term that means Kubernetes will decide which node each of the pods should run on.

The other advantage is that we can make our system tolerant to failure. For example we can create two instances of our API Gateway. The Kubernetes master node then will ensure that the API gateway is running on at least two nodes at a time even if one instance crashes.

And one last word as well, we're going to be able to set up
virtual hard drives in the cloud. In Amazon they're called EBS volumes for elastic block storage, which we will configure in our persistent volume claim.

### Managing a Cluster in the Cloud

So as outlined in the previous section, we're going to have a collection of nodes. And by the way, these are often also called _worker nodes_. And the job of the worker nodes is to run our pods. We're also going to have at least one further node called a master or _master node_.

In order to create a cluster, we're going to choose between the two tools _Kops_, and _EKS_. And because Kops and Eks are very popular, this course is going to cover both of them.


#### KOPS

kOps - Kubernetes Operations - is the older of the two choices. In fact, the very first edition of this course used Kops exclusively because at that time Eks wasn't available.

[KOPS](http://github.com/kubernetes/kops) is an official part of Kubernetes. They say it is...

>_The easiest way to get a production grade Kubernetes cluster up and running._

But what does it do?

>_We like to think of it as kubectl for clusters._
>_KOPS will not only help you create, destroy, upgrade and maintain production-grade, highly available, Kubernetes cluster, but it will also provision the necessary cloud infrastructure._

What does it support?

>_AWS (Amazon Web Services) is currently officially supported, with DigitalOcean, GCE, and OpenStack in beta support, and Azure and AliCloud in alpha._

#### Amazon EKS

But you're probably aware that there is now a big competitor out there, and this one is EKS, the Elastic Kubernetes Service. And the big deal about Eks really is that it is part of AWS itself.

#### Differences between Kops and Eks

There is one massive difference between the way you work with the two is related to this masternode. I'll put briefly, if you're working in Kops, you are responsible for managing that master node. Whereas if you're in Eks, you won't even see the master node. You have no visibility of it. It's running in the background, but it's protected by Amazon.

<div class="page"/>

## Section 14: KOPS - Running Kubernetes on the AWS Cloud

### 56. Introducing Kops - Kubernetes Operations

There is a link on the [Kops Github Page](https://github.com/kubernetes/kops) for [Getting Started with Kops on AWS](https://kops.sigs.k8s.io/getting_started/install/).

Now, you might be surprised to see that there are instructions here for macOS and Linux. That is because, we're working on the Amazon, but the idea is here, you can install KOPS onto your local development machine and then KOPS will talk to your EC2 account to start up the nodes and the master and any other resources that are required.

But we won't do that here, we will launch an EC2 instance and install Kops there. We create a t2-nano instance.

First we chose a `Amazon Linux 2 Ami (HVM), SSD volume type` image and for Instance Type we select `t2 nano` and on the Add Tags page we add a key `Name` and Value `bootstrap`. In Step 6 we configure the security group, where we can restrict access to Source `My Ip`.

Now we can click on `Review and Launch`. Here we need to create a key pair, which allows us to connect to our machine using ssh.

### 57. Installing the Kops Environment

Now we can install Kops to our EC2 instance. We use the instructions on []

```bash
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops
sudo mv kops /usr/local/bin/kops
```

In the next step we need to setup an IAM user.

In order to build clusters within AWS we'll create a dedicated IAM user for kops. This user requires API credentials in order to use kops. Create the user, and credentials, using the AWS console.

```bash
AmazonEC2FullAccess
AmazonRoute53FullAccess
AmazonS3FullAccess
IAMFullAccess
AmazonVPCFullAccess
```

Now we need to export the Access Key and the Secret for our user to login from our console.

Now we have to run the aws configure tool which will ask you for your Access Key and the Secret.

```bash
# configure the aws client to use your new IAM user
aws configure           # Use your new access and secret key here
aws iam list-users      # you should see a list of all your IAM users here

# Because "aws configure" doesn't export these vars for kops to use, we export them now
export AWS_ACCESS_KEY_ID=$(aws configure get aws_access_key_id)
export AWS_SECRET_ACCESS_KEY=$(aws configure get aws_secret_access_key)
```

Now we DON'T have to configure the DNS - Domain Name System - see [Configure DNS](https://kops.sigs.k8s.io/getting_started/aws/#configure-dns), since we use _gossip-based DNS_.

See the video [Installing the Kops Environment](https://www.udemy.com/course/kubernetes-microservices/learn/lecture/11157264#questions) and the official documentation [Getting Started  - Deploying to AWS](https://kops.sigs.k8s.io/getting_started/aws/).

### 60. Configuring your first cluster

In order to store the state of your cluster, and the representation of your cluster, we need to create a _dedicated S3 bucket_ for kops to use. S3 is just a distributed file system on the AWS.

You can use the command line tool or you can use the GUI and go to _Services > Storage > S3_  where you can create a new bucket.

The only complexity is you need to give this bucket a globally unique name. So, that means a name that nobody has ever used, in the history of Amazon S3.

#### Creating the cluster

We've now finally got to the point where we can create the cluster.

In order to do this we need to prepare our local environment. Let's first set up a few environment variables to make the process easier.

For a gossip-based cluster, make sure the name ends with `k8s.local`.

```bash
export NAME=fleetman.k8s.local
export KOPS_STATE_STORE=s3://chesterwood-state-storage
```

In order to create our cluster configuration we need the _availability zones_. An availability zone is a data centre owned by Amazon. On the AWS console if I go to services and EC2 Dashboard, we will find the required information. In each region, such as London, Amazon will be operating a number
of physically separate data centres such like `eu-west-2a`, `eu-west-2b`, and `eu-west-2c`.


Now this is particularly powerful because we're going to be deploy our system across multiple nodes. And it makes great sense, that each of the nodes is in a different data centre. So this would give us much stronger resilience when a particular availability centre is  going down.

When we create the configuration for the cluster, we tell KOPS which availability zones we want to distribute our system across.

In our configuration we will be deploying our cluster to the `eu-west-2` region.

```bash
aws ec2 describe-availability-zones --region eu-west-2
```

The below command will generate a new cluster configuration, but will not start building it.

```bash
kops create cluster --zones=eu-west-2a,eu-west-2b,eu-west-2c ${NAME}
    ${NAME}
```

The `Name` is the name we created above.

Now we need to create a rsa key pair.

```bash
ssh-keygen -b 2048 -t rsa -f ~/.ssh/id_rsa
```

```bash
kops create secret --name ${NAME} sshpublickey admin -f ~/.ssh/id_rsa.pub
```

This gives kops the privileges it needs to work with our cluster.

Now we could customize the cluster configuration but we'll use the default configuration.

Now we can configure the instance groups of our cluster.

```bash
kops edit ig nodes --name ${NAME}
```

### 61. Running the Cluster

We've set up the configuration of our cluster. Now we want to apply the configuration to real hardware.

```bash
kops update cluster ${NAME} --yes
```

This will take some time to create the cluster.

```bash
kops validate cluster
```

This will take about 5 minutes to start the cluster with all nodes.

If we go to `Load Balancing > Load Balancers` in the AWS GUI then we will see a new loadbalancer.


<div class="page"/>

## Section 15: EKS - Running Kubernetes on the AWS Cloud

This section is for those of you who have chosen to use EKS.

If you're using Kops, you can safely skip the section and continue with "Operating your Cluster".

### 63. Getting started with EKS

Login to AWS, search for the Service (_Find Services_) `EKS` in the _AWS Management Console_, and follow the Link. Choose a region that supports _EKS_.

There is a graphical interface and you might be thinking, that this sounds great, but it isn´t. But at the time of recording this is not an easy process. This is why we are going to use the command line and a tool called _eksctl_ to configure our cluster.

On the documetation for _EKS_ there is a page for [Getting started with eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html).

There is also a github page for `eksctl` - [weaveworks/eksctl](https://github.com/weaveworks/eksctl).

You can install it on your system, but it is much easier to install it inside the aws account. For this we create an _EC2 instance_.

We leave _EKS_ and switch to _Services_ in the menu, where we chose _Compute - EC2_. Here we launch a new _Instance_, which we call our _bootstrap instance_. We need to create an instance of an image called _Amazon Linux 2 AMI (HVM) SSD Volume Type_. In _Step 2_ we chose an _Instance Type_ called _t2.micro_. On Step 3 - Configure Instance Details - and Step 4 - Add Storage - the defaults are fine. On Step 5 - Add Tags - we add a Tag with key `Name` and value `Bootstrap`. In Steo 6 - Configure Security Group - we can configure a set of firewall rules, where we can restrict access to `SSH`, Protocol `TCP` and Port `22`. This is where you can restrict the `Source` to `My_IP` address. In Step 7 - Review Instance Launch - we have to configure a `Public Key` and `Private Key` which we download and call `eks-demo.pem`. Now you can launch the new Instance.

Now we can start a terminal and login using `ssh`, whereby we assume that the ip-address is `8.184.8.251`.

```bash
ssh -i eks-demo.pem ec2-user@8.184.8.251
```

### 64. Install eksctl and AWS CLI

First we will install `eksctl` itself. To download the latest release, run the following command which you find on the [eksctl github page](https://github.com/weaveworks/eksctl).

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

Now we should be able to run `eksctl`, the official command line interface (CLI) for Amazon EKS.

```bash
eksctl
```

The `eksctl` tool is issueing a lot of command based upon the `aws` command line interface. We have not installed the `aws` tool but it comes preinstalled on this image we are using as our bootstrap image.

But there is a problem withe the version of the `aws` tool. This shoul be version `1.18.86`, or version `2.0.25` or higher. To check this we use the following command.

```bash
aws --version
```

We update the AWS CLI to Version 2 in order to fulfill the requirements.

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Now log out of your shell and back in again.

### 65. Configure the AWS credentials

Before we use the `aws` tool we must configure the credentials.

We need to setup a user in our aws account and the login as this user.

In the AWS Management console we search for the service _iam_ the _Identity and Access Management_. Then we create a _Group_ which automatically creates a user.

We create a group _eksGroup_ and the _policy_ called _AmazonEC2FullAccess_, _IAMFullAccess_, and _AWSCloudFormationFullAccess_. The we have to add an additional policy. For this we click on the group and then click on _Inline Policies_ where we create a new one.

```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "eks:*",
      "Resource": "*"
    },
    {
      "Action": [
        "ssm:GetParameter",
        "ssm:GetParameters"
      ],
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```

For more information take a look at [Minimum IAM policies](https://eksctl.io/usage/minimum-iam-policies/).

Now we click on _User_ in the Management console to create a user called `eksUser`, click on _programatic access_ and add the user to the `eksGroup`. After finishing we can copy the users _Access Key ID_ and the `secret access key`.

Now we configure our `aws` tool using the credentials above and the region we want to use e.g. _eu-central-1_.

```bash
aws --configure`
```

Once we've got the cluster up and running, we just work with it using kubectl in exactly the same way we've been doing throughout this course. It's just that well, we haven't installed kubectl. So that's going to be the last job before we start our cluster.

### 66. Install kubectl

Amazon Eks is sometimes a version or two behind whatever the current version of Kubernetes is. So we do have a potential here that we might get a mismatch between our Kubernetes version and our kubectl version. So we have to find out the kubectl version of Amazon EKS.

For this we go to the _EKS dashboard_ and start creating a cluster, but we don't finish that. On the first page we find the actual version.

```bash
# export RELEASE=<enter default eks version number here. Eg 1.17.0>
export RELEASE=1.17.0
curl -LO https://storage.googleapis.com/kubernetes-release/release/v$RELEASE/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

Now check the version with `kubectl version --client`.

### 67. Start the cluster

Now we can create the cluster.

```bash
eksctl create cluster --name fleetman --nodes-min=3
```

This will last about 20 minutes to start the cluster.

<div class="page"/>

## Section 16 - Operating your cluster

Whether you've decided to use EKS or Kops, you now have a running cluster.

From now on, for most operations, you now work with the cluster in exactly the same way - you apply yaml files using `kubectl`.

In this section we will deploy the Fleetman application to your cluster.


### 69. Provisioning SSD drives with a StorageClass

First We'll start with the `storage.yaml` file. But we call this file `storage-aws.yaml`, because we are going to make some changes.

Previously we were using host path to store the data on a local directory structure outside of the container. Now that is not going to work in a clustered environment.

In fact there is a better way, and the better way is to replace this configuration block, with a new configuration block called a _storage class configuration_.

A [storage class](https://kubernetes.io/docs/concepts/storage/storage-classes/) allows us to define a type of storage, and the beauty of these storage classes is they will be dynamically created as and when they're needed.

Here on the kubernetes reference manual, there is a page on [storage classes]. If we go a little further down this document, there's a section here called provisioner, and this is going to be one of the configuartion fields in this document.

There's a long list of different provisioners, one of which is [AWS elastic blockstore](https://kubernetes.io/docs/concepts/storage/storage-classes/#aws-ebs).

And then when we make one of these persistent volume claims, we could use that slow or fast here, to say what type of storage that we need.

Now I'm going to delete everything, and add our new configuration.

```bash
# What do want?
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-pvc
spec:
  storageClassName: cloud-ssd
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 7Gi
---
# How do we want it implemented? Here AWS gp2 ssd
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: cloud-ssd
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
```

Take care that `storageClassName` must match the `metadata: name:...`.

Now just activate that configuration.

```bash
kubectl apply -f storage-aws.yaml`
```

And check the configuration for the persistence volume and claim.

```bash
kubectl get pvc
kubectl get pv
```

### 71. Deploying the Fleetman Workload to Kubernetes

Now we can test if our storage configuration works and create the `mongo-stack.yaml` file. We create the file using nano and copy the content into the file.

In order to apply the configuration we use the following command.

```bash
kubectl apply -f mongo-stack.yaml
```

It can take a while until the mongo database is created.

```bash
kubectl describe pod/mongodb-df864976d-k74xn
```

And we can check if we find `MountVolume.Setup succeeded`. If this is the case we are fine.

We can execute the following commands.

```bash
kubectl get all
kubectl -f logs pod/mongodb-df864976d-k74xn
```

Now we create the `workloads.yaml` and apply the configuration. 

We also create the `services.yaml` where we have to change `NodePort` to `LoadBalancer` for the webapp service and change the type to `ClusterIP` for the position tracker.

Now we can do a `kubectl -f .` and then check the position tracker.

```bash
kubectl -f .
kubectl logs -f pod/position-tracker-57c68834-zkbvf
```

In order to check if our loadbalancer is available we go to the AWS management console. There should be two loadbalancers if we use kops and only one if we use eks. On the _description tab_ we can find the _DNS name_ for our service, we could use to check if our app is up and running. This dns name looks a little bit ugly, e.g. `a5bc778cdf95abda678ffg43.eu-west-2.eib.amazonaws.com`.

### 72. Setting up a real Domain Name

This is a simple process.

### 73. Surviving Node Failure

We can get more information about our pods if we use the `-o wide` option.

```bash
kubectl get all -o wide
```

This gives us the information on which node the pods are running on.

<div class="page"/>

## Section 17: Deleting the Cluster in Kops

### 75. Deleting the Cluster

```bash
kops delete cluster --name ${NAME} --yes
```

It is important that you let that script running until it is finished, otherwise you run into problems.

### 76. Restarting the Cluster

Your first step is to go to the AWS Management Console and restart your bootstrap instance.

One thing you have to check is in your `Security groups` for this boostrap and on the _Inbound tab_ and when you have a dynamic ip-address, the you have to click on it and change it to your actual ip-address _my_ip_.

## Section 18: Deleting the Cluster in EKS

### 77. How to delete your EKS cluster

As you know, it's very, very important that you must delete your cluster when you're finished. Otherwise, you will incur ongoing costs. To do that, we will use the eksctl tool. It's a very simple command.

```bash
eksctl delete cluster fleetman
```

Another thing to check though is I'll switch across to the Eks page. You can find that by dropping down the services link, type in Eks and follow the link there. If you click on the clusters link, we want to see we now do not have any clusters now on this 

One thing that has not been deleted automatically, however, is the volume. You'll remember that we use some persistent storage to store our Mongo database inside.

## Section 19: Extra - how to run Kubernetes in Google Cloud

## 78. How to deploy to Google Cloud Platform

As you know, the course was designed around AWS, as that is by far the most in-demand cloud platform, and it also happens to be the one I use in my real work. It's the platform I know and trust.

But I've been asked by a few people if I can show how to run the application on Google Cloud Platform. It was quite a challenge as I've hardly ever used the platform before - so I spent a day working on porting the Fleetman application to GCP.

[Get Access to the video here!](https://www.youtube.com/watch?v=w9vy3wHGKeo)

## Section 20 - Logging a Kubernetes Cluster

### 79. Introducing the ELK / ElasticStack

The ELK Stack - now called ElasticStack - is called a stack because it consists of multiple software components.
It consists of three major distributed components/parts:

- ElasticSearch
- Logstash or Fluentd
- Kibana

Logstash/Fluentd

- is able to find the logs from the several docker containers and grabs them
- is not able to store the logs
- to store the logs another component is necessary
- sends the data to ElasticSearch
- has to be installed on every single node of the cluster

ElasticSearch

- is a distributed, RESTful search and analytics engine
- is used to centrally store the logs or your data so you can discover the expected and uncover the unexpected
- has to be replicated since there should always be one instance of it that stores the data

Kibana

- visualizes the data from ElasticSearch

### Installing the stack

Kubernetes has some preconfigured _yaml_-files for ElasicSearch together with Fluentd - see <https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch>.
It uses a _DaemonSet_ to assure that _fluentd_ is started on each node - see the _elasic_stack.yaml_ file provided by Dick Chesterwood which combines the kuberntes files:

```yaml
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-es-v2.2.0
  namespace: kube-system
  labels:
    k8s-app: fluentd-es
    version: v2.2.0
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
spec:
  selector:
    matchLabels:
      k8s-app: fluentd-es
      version: v2.2.0
  template:
    metadata:
      labels:
        k8s-app: fluentd-es
        kubernetes.io/cluster-service: "true"
        version: v2.2.0
      # This annotation ensures that fluentd does not get evicted if the node
      # supports critical pod annotation based priority scheme.
      # Note that this does not guarantee admission on the nodes (#40573).
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ''
        seccomp.security.alpha.kubernetes.io/pod: 'docker/default'
    spec:
      priorityClassName: system-node-critical
      serviceAccountName: fluentd-es
      containers:
      - name: fluentd-es
        image: k8s.gcr.io/fluentd-elasticsearch:v2.2.0
        env:
...
```

<div class="page"/>

## Section 21: Monitoring a Kubernetes Cluster with Prometheus and Grafana

You want to get some answers to questions you ask when you are monitoring your webapps.

- are your nodes healthy:
- are they consuming the right levels of cpu?
- are there enough nodes?

### 86. Installing the kube-prometheus-stack

Since the Helm Chart for Prometheus Operator is now deprecated, we move away from this. The new Chart is called _kube-prometheus-chart_.
### Monitoring a cluster

Prometheus

- is an open source monitoring solution
- is capable of gathering information of a kubernetes cluster
- has only a basic user interface
- you can plug-in a graphical tool of your choice

Grafana

- is an open source graphical tool/frontend

### Helm Package Manager

The Helm Package Manager

- is the package manager for kubernetes
- is like apt-get or brew
- offers an easy way to install & upgrade Kubernetes apllications like Prometheus & Grafana
- allows to install a collection of pods & services

Cloud Native Computing Foundation

### Prometheus

### Working with Grafana

List everything in namespace _monitoring_:

```bash
kubectl get all -n monitoring
```

Edit the configuration of a service:

```bash
kubectl edit -n monitoring service/monitoring-prometheus-oper-prometheus
```

You can switch the _type_ from _ClusterIp_ to _LoadBalancer_ to have access to the service.

### Kubernetes Dashboard - Web UI

Install the dashboard - see <https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/>.
Deploying the Dashboard UI:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

You can access Dashboard using the kubectl command-line tool by running the following command:

```bash
kubectl proxy
```

Kubectl will make Dashboard available at <http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/.>
You can sign in with a token - see <https://stackoverflow.com/questions/46664104/how-to-sign-in-kubernetes-dashboard>

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | awk '/^deployment-controller-token-/{print $1}') | awk '$1=="token:"{print $2}'
```

For an overview about authentication see <https://kubernetes.io/docs/reference/access-authn-authz/authentication/.>

### Follow Up

Docker for Mac with Kubernetes — Ingress Controller with Traefik <https://medium.com/@thms.hmm/docker-for-mac-with-kubernetes-ingress-controller-with-traefik-e194919591bb>

<div class="page"/>

## Storing Passwords and Secrets in a secure way

### Secrets

Create Password/Secret

```bash
kubectl create secret generic mysql-pass --from-literal=password=DockerCon
```

Will be exposed as an environment variable in the container.

```bash
kubectl get secrets
```

### Deployment to get the secret

```yaml
   ...
   env:
   - name: MYSQL_ROOT_PASSWORD
     valueFrom:
       secretKeyRef:
         name: mysql-pass
         key: password
   ...
```

Create the deployment

```bash
kubectl create -f mysql-deployment.yaml
```

<div class="page"/>

## Ingress Controllers

### Install Ingress Addon in Minikube

```bash
minikube addons list
```

```bash
minikube addons enable ingress
```

```bash
kubectl get po -n kube-system
```

### Fake a Domain Name

Before you can use your _Ingress_ you have to fake a domain name.

If you use Minikube, you have to find out the ip address of your minikube and add a faked domain name to your `/etc/hosts`file.

```text
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
127.0.0.1       fleetman.com
```

If you don`t use minikube on mac, then you can use the localhost's ip address.

> Note: This does not work for me - Maybe this will help <https://github.com/kubernetes/minikube/issues/5494> (Enable host OS dns resolution of cluster ingress host names)?

### Configure your Ingress

You can create a file that is called `ingress.yaml`.

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: basic-ingress
spec:
  rules:
  - host: fleetman.com
    http:
      paths:
      - path: /
        backend:
          serviceName: fleetman-webapp
          servicePort: 80
```

If you want to find out what the port is, that your webapp uses.

```bash
kubectl get svc
```

Get some info for the configured ingress.

```bash
$ kubectl get ingress
NAME                HOSTS    ADDRESS     PORTS   AGE
thymeleaf-ingress   t8s.io   localhost   80      3h52m
```

```bash
$ kubectl describe ingress thymeleaf-ingress
Name:             thymeleaf-ingress
Namespace:        default
Address:          localhost
Default backend:  default-http-backend:80 (<none>)
Rules:
  Host  Path  Backends
  ----  ----  --------
  *      /   thymeleaf-tour:80 (10.1.0.106:8080,10.1.0.107:8080)
Annotations:
  kubectl.kubernetes.io/last-applied-configuration:  {"apiVersion":"networking.k8s.io/v1beta1","kind":"Ingress","metadata":{"annotations":{},"name":"thymeleaf-ingress","namespace":"default"},"spec":{"rules":[{"host":"t8s.io"},{"http":{"paths":[{"backend":{"serviceName":"thymeleaf-tour","servicePort":80},"path":"/"}]}}]}}
```

### Add Authentication

TODO:

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: basic-ingress
spec:
  rules:
  - host: fleetman.com
    http:
      paths:
      - path: /
        backend:
          serviceName: fleetman-webapp
          servicePort: 80
```

### Installation Guide for NGINX Ingress

This is a short guide on how to install Ingress on Docker Desktop and AWS For more information take a look at the full [Installation Guide](https://kubernetes.github.io/ingress-nginx/deploy/).

#### Prerequisite Generic Deployment Command

The following Mandatory Command is required for all deployments.

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
```

After that you have to follow the provider specific instructions.

#### Docker Desktop

Kubernetes is available in Docker for Mac (from version 18.06.0-ce). To create a service apply the following configuration.

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/cloud-generic.yaml
```

Check if the ingress-nginx pods is running.

```bash
$ kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx --watch
NAMESPACE       NAME                                        READY   STATUS    RESTARTS   AGE
ingress-nginx   nginx-ingress-controller-5974f8b65b-qmtxk   1/1     Running   0          167m
```

#### AWS specific stuff

In AWS you will use an Elastic Load Balancer (ELB) to expose the NGINX Ingress controller behind a Service of Type=LoadBalancer.

There are two possibilites to install Ingress.

This setup requires to choose in which layer (L4 or L7) we want to configure the ELB:

- Layer 4: use TCP as the listener protocol for ports 80 and 443.
- Layer 7: use HTTP as the listener protocol for port 80 and terminate TLS in the ELB

We use the Layer 4 (TCP) version.

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/aws/service-l4.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/aws/patch-configmap-l4.yaml
```

Check <https://kubernetes.github.io/ingress-nginx/deploy/#aws> for more instructions.

The result will be that we have a Loadbalancer at port 80, that refers to the ingress controller, which fan outs to multiple services.

<div class="page"/>

## More Stuff

### Docker for Mac

Kubernetes / Ingress is available in Docker for Mac (from version 18.06.0-ce)

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/cloud-generic.yaml
minikube
```

### Dockerhub

#### Add your image to docker hub

You may push your image to a new repository using the CLI.

```bash
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```

```bash
docker tag thymeleaf-tour:v0.1 thymeleaf-tour:v0.1
docker push thymeleaf-tour:v0.1
```

### Setting up Ingress with Docker Desktop's Kubernetes

The following instructions apply to both Docker for Mac and Docker for Windows, even though the docs only list Docker for Mac. See [260. Setting up Ingress with Docker Desktop's Kubernetes](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/15603106#overview)

Execute the provider-specific command noted here:
https://kubernetes.github.io/ingress-nginx/deploy/#docker-for-mac

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/deploy/static/provider/cloud/deploy.yaml
```

Verify the service was enabled by running the following:

```bash
kubectl get pods -n ingress-nginx
```

It should show something similar:

```bash
ingress-nginx-controller-5cc4589cc8-x7pg2   1/1     Running     0          66s
```

Let's create a configuration for our ingress-nginx in a file called `ingress-service.yaml`.

```yaml
apiVersion: networking.k8s.io/v1beta1
# UPDATE THE API
kind: Ingress
metadata:
  name: ingress-service
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/use-regex: 'true'
    # ADD THIS LINE ABOVE
    nginx.ingress.kubernetes.io/rewrite-target: /$1
    # UPDATE THIS LINE ABOVE
spec:
  rules:
    - http:
        paths:
          - path: /?(.*)
          # UPDATE THIS LINE ABOVE
            backend:
              serviceName: client-cluster-ip-service
              servicePort: 3000
          - path: /api/?(.*)
          # UPDATE THIS LINE ABOVE
            backend:
              serviceName: server-cluster-ip-service
              servicePort: 5000
```

The ingress controller supports case insensitive regular expressions, see [Ingress Path Matching](https://kubernetes.github.io/ingress-nginx/user-guide/ingress-path-matching/) for more informantion.

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: test-ingress-3
  annotations:
    nginx.ingress.kubernetes.io/use-regex: "true"
spec:
  rules:
  - host: test.com
    http:
      paths:
      - path: /foo/bar/bar
        backend:
          serviceName: test
          servicePort: 80
      - path: /foo/bar/[A-Z0-9]{3}
        backend:
          serviceName: test
          servicePort: 80
```


<div class="page"/>

## Tipps and Tricks

### Kubectl autocomplete

Set autocomplete for bash or zsh.

Bash
```bash
source <(kubectl completion bash)
```

Zsh

```bash
source <(kubectl completion zsh)
```

## Links & further information

### Install Kubernetes Dashboard

See [https://github.com/kubernetes/dashboard/](https://github.com/kubernetes/dashboard/)

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```

More Information about the dashoboard

- <https://dzone.com/articles/how-to-install-the-kubernetes-dashboard>
- <https://kubernetes.io/docs/tutorials/hello-minikube/>

- <https://mattperrins.wordpress.com/2018/01/06/setup-docker-for-mac-with-kubernetes-and-install-a-spring-microservice-from-ibm-cloud/>
- <https://medium.com/javarevisited/kubernetes-step-by-step-with-spring-boot-docker-gke-35e9481f6d5f>
- <https://rominirani.com/tutorial-getting-started-with-kubernetes-with-docker-on-mac-7f58467203f>

```bash
Name:         monolith
Namespace:    default
Priority:     0
Node:         gke-io-default-pool-6847824b-n5xx/10.128.0.3
Start Time:   Sat, 06 Jun 2020 23:33:46 +0200
Labels:       app=monolith
Annotations:  <none>
Status:       Running
IP:           10.4.1.4
IPs:          <none>
Containers:
  monolith:
    Container ID:  docker://3f49e4f08b64d0099f8f84bb259073bec7e4ee8b6ee75d45b761c62ddba4df18
    Image:         kelseyhightower/monolith:1.0.0
    Image ID:      docker-pullable://kelseyhightower/monolith@sha256:72c3f41b6b01c21d9fdd2f45a89c6e5d59b8299b52d7dd0c9491745e73db3a35
    Ports:         80/TCP, 81/TCP
    Host Ports:    0/TCP, 0/TCP
    Args:
      -http=0.0.0.0:80
      -health=0.0.0.0:81
      -secret=secret
    State:          Running
      Started:      Sat, 06 Jun 2020 23:33:49 +0200
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     200m
      memory:  10Mi
    Requests:
      cpu:        200m
      memory:     10Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-rpg77 (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-rpg77:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-rpg77
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From                                        Message
  ----    ------     ----  ----                                        -------
  Normal  Scheduled  43s   default-scheduler                           Successfully assigned default/monolith to gke-io-default-pool-6847824b-n5x
x
  Normal  Pulling    42s   kubelet, gke-io-default-pool-6847824b-n5xx  Pulling image "kelseyhightower/monolith:1.0.0"
  Normal  Pulled     41s   kubelet, gke-io-default-pool-6847824b-n5xx  Successfully pulled image "kelseyhightower/monolith:1.0.0"
  Normal  Created    40s   kubelet, gke-io-default-pool-6847824b-n5xx  Created container monolith
  Normal  Started    40s   kubelet, gke-io-default-pool-6847824b-n5xx  Started container monolith
student_04_e2a6f513c2f6@cloudshell:~/orchestrate-with-kubernetes/kubernetes (qwiklabs-gcp-04-e14903d00b3e)$
```

Use the `gcloud compute firewall-rules` command to allow traffic to the monolith service on the exposed nodeport:

```bash
student_04_e2a6f513c2f6@cloudshell:~/orchestrate-with-kubernetes/kubernetes (qwiklabs-gcp-04-e14903d00b3e)$ gcloud compute firewall-rules create 
allow-monolith-nodeport \
>   --allow=tcp:31000
Creating firewall...⠹Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-04-e14903d00b3e/global/firewalls/allow-monolith-nodepor
t].
Creating firewall...done.
NAME                     NETWORK  DIRECTION  PRIORITY  ALLOW      DENY  DISABLED
allow-monolith-nodeport  default  INGRESS    1000      tcp:31000        False
student_04_e2a6f513c2f6@cloudshell:~/orchestrate-with-kubernetes/kubernetes (qwiklabs-gcp-04-e14903d00b3e)$
```


```bash
student_04_e2a6f513c2f6@cloudshell:~/orchestrate-with-kubernetes/kubernetes (qwiklabs-gcp-04-e14903d00b3e)$ gcloud compute instances list

NAME                               ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
gke-io-default-pool-6847824b-b3g3  us-central1-b  n1-standard-1               10.128.0.4   34.68.252.36   RUNNING
gke-io-default-pool-6847824b-l60n  us-central1-b  n1-standard-1               10.128.0.2   35.239.57.229  RUNNING
gke-io-default-pool-6847824b-n5xx  us-central1-b  n1-standard-1               10.128.0.3   35.184.99.91   RUNNING
student_04_e2a6f513c2f6@cloudshell:~/orchestrate-with-kubernetes/kubernetes (qwiklabs-gcp-04-e14903d00b3e)$
```

```bash
TOKEN=$(curl http://127.0.0.1:10080/login -u user|jq -r '.token')
```

```bash
student_04_e2a6f513c2f6@cloudshell:~ (qwiklabs-gcp-04-e14903d00b3e)$ kubectl get all
NAME                            READY   STATUS    RESTARTS   AGE
pod/auth-789956d4f9-7rbsb       1/1     Running   0          20m
pod/frontend-5dd6cbb4dd-qpsgt   1/1     Running   0          18m
pod/hello-56ff65f9bf-hwk9g      1/1     Running   0          19m
pod/hello-56ff65f9bf-k99pj      1/1     Running   0          19m
pod/hello-56ff65f9bf-p5b9q      0/1     Pending   0          19m
pod/monolith                    1/1     Running   0          52m
pod/nginx-574c99d7c8-sfxmt      1/1     Running   0          56m
pod/secure-monolith             2/2     Running   0          38m
NAME                 TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)         AGE
service/auth         ClusterIP      10.7.251.152   <none>           80/TCP          20m
service/frontend     LoadBalancer   10.7.247.6     104.198.199.28   443:31189/TCP   18m
service/hello        ClusterIP      10.7.247.220   <none>           80/TCP          19m
service/kubernetes   ClusterIP      10.7.240.1     <none>           443/TCP         59m
service/monolith     NodePort       10.7.252.44    <none>           443:31000/TCP   33m
service/nginx        LoadBalancer   10.7.245.142   35.225.182.69    80:31134/TCP    55m
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/auth       1/1     1            1           20m
deployment.apps/frontend   1/1     1            1           18m
deployment.apps/hello      2/3     3            2           20m
deployment.apps/nginx      1/1     1            1           56m
NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/auth-789956d4f9       1         1         1       20m
replicaset.apps/frontend-5dd6cbb4dd   1         1         1       18m
replicaset.apps/hello-56ff65f9bf      3         3         2       20m
replicaset.apps/nginx-574c99d7c8      1         1         1       57m
student_04_e2a6f513c2f6@cloudshell:~ (qwiklabs-gcp-04-e14903d00b3e)$```