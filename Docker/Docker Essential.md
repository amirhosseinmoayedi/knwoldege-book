#docker 
## What are <font color="#31859b">Containers</font>?
 A group of processes run in **isolation** , All processes MUST be able to run on the **shared kernel**.
### Each container has its own set of "<font color="#ffff00">namespaces</font>" (<font color="#e36c09">isolated view</font>)
* PID- process IDs
* USER user and group IDs
* UTS hostname and domain name
* NS mount points
* .NET Network devices, stacks, ports
* IPC inter-process communications, message queues
<font color="#ffff00">cgroups</font> controls limits and monitoring of resources.

<font color="#ffff00">PID</font> is just one of the<font color="#ffff00"> Linux namespaces</font> that provides containers with **isolation to system resources**. Other Linux namespaces include:
* MNT: Mount and unmount directories without affecting other namespaces.
* NET: Containers have their own network stack.
* IPC: Isolated interprocess communication mechanisms such as message queues.
* User: Isolated view of users on the system. 
* UTC: Set hostname and domain name per container.
**These namespaces provide the isolation for containers that allow them to run together securely and without conflict with other containers running on the same system**.
<hr>
## What is a <font color="#92d050">Runtime</font>
A runtime is the **environment in which an application or program runs**. It provides the<font color="#92d050"> necessary infrastructure for executing the code</font>, including **managing memory allocation, handling input/output operations, and managing system resources**. The runtime defines the operating environment for the application, including the system libraries, language interpreter, and other dependencies.
## What is <font color="#0070c0">Docker</font> 
Docker provides a **runtime environment for containers**, enabling developers to run and manage their applications in a lightweight and portable environment. The Docker runtime allows for the creation, deployment, and execution of containers, which can contain applications and their dependencies.

<hr>
## How It's Work 

 > Tool makes it possible to run Docker containers on operating systems other than Linux is **Linux KIt**

**Windows containers can be run <font color="#31859b">only on</font> windows os.**

In enterpirse companies their will have **their own docker registery**(*which is a container it self*) for pulling and pushing created images.

## <font color="#953734">Union File System</font> :
<u>merge image layers into single file system for each container</u> .Each of these lines(in docker file) is a **layer**. Each layer contains **only the delta, or changes from the layers before it**. To put these layers together into a single running container, **Docker uses the union file system to overlay layers transparently into a single view.**

## <font color="#953734">copy-on-write</font>:
Copy-on-Write (CoW) is a technique used by Docker to manage file systems **within containers**. When a container is created, it starts with a base image and a new file system layer is added on top of it. <u>When a file in the container is modified, rather than modifying the original file, a new copy of the file is created and the changes are written to this new copy</u>. This allows multiple containers to share the same base image and **save disk space**. When a container is **deleted, only the changes made to the file system are discarded, not the entire base image**.

**Each layer of the image is read-only except for the top layer, which is created for the container**. The read/write container layer implements "copy-on-write," which means that **files that are stored in lower image layers are pulled up to the read/write container layer only when edits are being made to those files**. Those changes are then **stored in the container layer**.

![lab2-step6-understanding_image_layers_1.png](../../../_resources/lab2-step6-understanding_image_layers_1.png)

> This two options make it possible for the docker for not building new images layer and just change the the layer that is different ,saves time and space.

> A **smaller base image** means it will download (deploy) much faster, and it is also more secure because it has a **smaller attack surface**.

<hr>
## Image <font color="#76923c">Tagging</font> 
Docker image tags are used to **identify a specific version of an image**. When you push an image to a registry, you can tag it with a version number or descriptive label. This allows you to manage **multiple versions of an image** and <u>easily roll back to a previous version if needed</u>. Image tags also help in organizing images and keep track of their updates. By tagging images, you can specify **which image to use in a deployment and manage the image lifecycle**. Additionally, **tagging images with descriptive labels makes it easier for others to understand the purpose and version of an image when collaborating on a project**.

You can inspect **which files have been pulled up to the container level** with the `docker diff` command.

To look more closely at layers, you can use the `docker image history` command of the image you created.
