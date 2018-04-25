# Docker Tutorial:

# VM vs. Docker Containers:


### VMs:

**hypervisor**: a piece of software, firmware, or hardware that VMs run on top of. Hypervisor usually runs on physical computers that are called the *"host machine"*. The host machine provides VMs with resources like RAM and CPU. 

VM that is running on the host mahine is also called a *"guest machine"*. Contains application, and what  is needed to run the application (system bins and libs). 

Guest machines can be run either on a **hosted hypervisor** or a **bare-metal hypervisor**. 

**Hosted Hypervisor**: runs on the operating system of the host machine. Example OSX can have a VM (virtualBox or VMware Workstation8) installed on top of OSX. The VM cannot access the hardware, so it needs to access it through OSX.

**pros:** underlying hardware is less important. Tends to have more "hardware compatibility"
**cons:** more computing and resources spent for communicating with the host operating system than a direct communication hence lower performance/tradeoff

**Bare Metal Hypervisor**: tackeles the performance by installing and running on the host machine hardware. Interfaces directly with the underlying hardware. 

**pros:** results in higher performance, scalability, and stability
**Cons:** not as compatible because it will have to have specifics required for it to run.


![](https://cdn-images-1.medium.com/max/1600/1*RKPXdVaqHRzmQ5RPBH_d-g.png)

![](https://cdn-images-1.medium.com/max/1000/1*V5N9gJdnToIrgAgVJTtl_w.png)


**Container**:

Big differene between container and VM is that the containers **share** the host system's kernel with other containers. 

Each container gets its own isolated user space to allow multiple containers to run on a single host machine. 


**DOCKER**

Open-souorce project based on Linux containers.

1. Easy to use
2. Speed
3. Docker Hub
4. Modularity and Scalability


**Docker Concepts**

![](https://cdn-images-1.medium.com/max/800/1*K7p9dzD9zHuKEMgAcbSLPQ.png)

**Docker Engine**:
Layer on which Docker runs. 
1. Docker Daemon that runs in the host computer
2. Docker client that communicates with the DOcker Daemon to execute commands
3. A REST API for interacting with the Docker Daemon remotely

**Docker Client**:
What you as the end-user of DOcker communicate with...
Communicates my instructions to the docker dameon

**Docker Daemon**
Executes the commands snet to the docker client, runs on the host machine, but host never talks with it. 

**Dockerfile**

Where you write the instructions to build a Docker image:

RUN apt-get y install some-package
Expose 8000
ENV ANT_HOME /usr/local/apache-ant 


**Docker Image**
Images are read-only templates that get build based on the *dockerfile*. They define both what you want the application and its dependencies to looklike **and** what processes to run when it's launched.

Each instruction in the *dockerfile* adds a new "layer" to the image, which layers representing a portion of the image file system that either adds to or replaces the layer below it. Layers are key to docker's lightweight yet powerful structure. 

**Union File Systems**:
build up an image
think of it as a stackable file system, meaning files and directories of separate file systems (brnaches) can be transparently overlaid to form a single file system. 

***Benefits of a Layered System:***
- Duplication-free: avoids duplicating a complete set of files every time you use an image to create and run a new container. Makes instantiation of a docker container very fast and cheap
- Layer Segregation: Making a change is much faster when you change an image, Docker only needs to propagate the updates to the layer that was changed

**Volumes:** 
"Data" part of a container, initialized when a container is created. Allows you to persist and share a container's data.. Data volumes are separate from the default union file system and exist as normal directories and files on the host filesystem. Even if you update, destroy, or rebuild your container, the data volumes will remain untouched. You make changes to it directly. 

![](https://cdn-images-1.medium.com/max/800/1*hZgRPWerZVbaGT8jJiJZVQ.png)

**Containers** 
Wraps an application's software into a box with everything the application needs to run. When created, docker creates a network interface so that the container can talke to the local host. 

EXAMPLES:

**Namespaces**:
Provide containers with their own view of the underlying Linux System. When running a container, docker creates namespaces that the specific container will use.

- NET: own view of the network stack of the system. 
- PID: Process ID ps aux what processes are running on the system
- MNT: gives a container their own view of the mounts on the system
- UTS: identify system identifiesrs(hostname, domainname)
- IPC: Responsible for isolating IPC resources between processes running inside each container
- USER: Namespace is used to isolate users within each container.

**Control Groups (cgroups)**
isolates, prioritizes, and accounts for the resource usage. Ensures that docker containers don't exhaust resources






## WHO WINS?

* If you need multiple applications on multiple servers it makes sense to use VMs 
* Run many *copies* of a single application, it makes sense to runn docker

Container breaks app into functional discrete parts, it also means there's a growing number of parts to manage.

Docker is build to deploy applications, not machines. Never install all services into one container, you want them all in separate nodes that distinguish between many. 

Client server arcitecture -> server uses the docker engine to cache images, run and manage containers, handle logs and much more

------


## Understanding the breakdown of the code:

- Docker wil copy the directory's content into a temporary directory and use that as context
- Docker retrieves the latest ubuntu and all of its intermediate images
- Docker runs the command inside the container and route standard output and standard error to our machiens so that we can see the result


*Every command that could potentially alter the state of the image, produces an "intermediate image" -> every time such a step is encountered an image will be created that holds the state produced by all the previous commands*

```{bash}
FROM ubuntu
WORKDIR /tmp
COPY test.txt .
RUN cat test.txt
```

Code above will produce 4 intermediate images:
Each line gets its own intermediate image.

When building an image, 

The final output of the building will allow you to run something like '29834798759238' and thats hard to call every time. 

Therefore: you can use something called tags>

`docker build -t test .`

You end up creating an image named **test** with the tag **latest**. Actual "tag" is the pair: <NAME>:<VERSION> -> which would become `test:latest`



------


## SOME THINGS TO KNOW:

* Can an image container contain  more than on container?
	*No, one image for one container*

* Can a container contain more than one process?
	*Yes, like apache + sshd, and in docker world, prefer to service/app for one container*

* Can I launch multiple containers from a single image?
	*Yes, it is like one binary files can be started several times with different process. In docker*

* Can I launch an operating system from an image?
	*yes, and it will be the same kernel with the docker host*

- Kernel is the middleman for the hardware and the software of a computer. Ex: unix, mac os, microsoft windows

* How can multiple containers be launched for one image?
	*when docker is launched, the real instance is called container. LIke a linux command. 

* Docker image OS must be the same OS as the host OS

*











