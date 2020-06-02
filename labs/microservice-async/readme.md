# Setting Up the Asynchronous Version of the Fortune-Cookies MOA to Run Under Kubernetes

(View the architecture diagram [here](architecture.md).)

The purpose of the version of Fortune Cookies is to provide hands-on experience creating a completely event driven MOA
under Kubernetes. The project uses `redis` as the backing service for Message Brokerage.

This set up process does the following:

* Takes you to the Katacoda interactive learning environment where you'll do your work.
* Installs a Docker registry locally in the Katacoda environment.
* Populates the local Docker registry with the container images you'll need. Some of the container images are built directly from
the source code in this project.
* Creates the Kubernetes pods and services that make up the asynchronous MOA using container images
stored in the local Docker registry and out on DockerHub.
* Shows you how to inspect the MOA by querying the logs of various pods in the Kubernetes cluster running in Katacoda.

## Setting up the K8S environment

**Step 1**: Go to the Katacoda K8S interactive learning environment for kubernetes `minikube`

`https://katacoda.com/javajon/courses/kubernetes-fundamentals/minikube`

**Step 2**: Clone the GitHub repo that contains the code

`git clone https://github.com/jruels/modern-cloud-bootcamp`

**Step 3**: Navigate into the lab directory

`cd modern-cloud-bootcamp/labs/microservice-async/`

**Step 4**: Create a local Docker registry and put the relevant container images in the registry

`sh docker-seed.sh`

**Step 5**: Generate the Kubernetes pods and services

`cd kubernetes`

`sh ./generate-k8s-resources.sh`

**Step 6**: Go into the `pingrx` pod so we can inspect the state of of things from inside the Kubernetes cluster. 
(`pingrx` is a utility I made that allows us to `ping` services internally
in the Kubernetes cluster. Also, you'll install `redis-tools` in the `pingrx`
container to add more debugging capabilities.)


`kubectl exec -it pingrx -- sh`

**Step 7**: Install `redis-tools` in the `pingrx` container.

`apt-get update && apt-get install redis-tools -y`

**Step 8**: Connect to the `redis` service running under Kubernetes using the `redis-cli` tool.

`redis-cli -h redis-master -p 6379`

**Step 9**: Ping the `redis` installation

`PING`

You should see a result, `PONG`

**Step 10**: Publish a message to `redis` just to make sure all is well.

`PUBLISH testChannel "Hi There!"`

You should see a return value similar to the following:

`(integer) 0`

**Step 11**: Exit out of the `redis` shell:

`exit`

**Step 12**: Exit out of the container

`exit`

**Step 13** Check the `sender` pod for activity

`kubectl logs sender -f`

**Step 14** Check the `scheduler` pod for activity

`kubectl logs scheduler -f`

**Step 15** Check the `fortunes` pod for activity

`kubectl logs fortunes -f`

**CONGRATULATIONS!**
