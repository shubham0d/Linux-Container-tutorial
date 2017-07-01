Linux Containers

Altough Containers get used by people due to extensively popularity of docker by Docker Inc but linux also have their Containers from several years.
These native linux Containers are named LXC(or simply linux container). LXC project has been their since 2008 and is actively being supported/sponsered by Cannonical Ltd. It is more of set of tools(more technically say API) which gives you interface to create a container using linux namespaces and cgroups(Control Groups).Linux namespaces(mnt,pid,net,ipc,uts, user) is a feature of kernel used for different type of isolation whereas cgroups are used to set limitation over resources(mostly hardware resources) use by any process.

Mount namespace: A process views different mount points other than the original system mount point. It creates a separate file system tree associated with different processes, which restricts them from making changes to the root file system.
PID namespace: PID namespace isolates a process ID from the main PID hierarchy. A process inside a PID namespace can have the same PID as a process outside it, and even inside the namespace, you can have different init with PID 1.
UTS namespace: In the UTS (UNIX Timesharing System) namespace, a process can have a different set of domain names and host names than the main system. It uses sethostname() and setdomainname() to do that.
IPC namespace: This is used for inter-process communication resources isolation and POSIX message queues.
User namespace: This isolates user and group IDs inside a namespace, which is allowed to have the same UID or GID in the namespace as in the host machine. In your system, unprivileged processes can create user namespaces in which they have full privileges.
Network namespace: Inside this namespace, processes can have different network stacks, i.e., different network devices, IP addresses, routing tables, etc.

LXC is a set of tools that make it possible to use these feature and create something that called as a container.Containers are nothing but a very lightweight VMs with less amount of isolation.Less isolation cause in container due to absent of hypervisor layer, which make container to use the same kernel of host system. But this absence also make container's more widely useble as the overhead of configuring hypervisor has gone and now you can select which resources you wish to isolate according to your usecase.So lets dive into LXC.

Getting Started with LXC
Note: For our demonstration purposes I will use LXC in Ubuntu 16.04. LXC has greate support on Ubuntu since LXC project is backed by Cannonical, whome is also 
the publisher of Ubuntu operating system. But you can also install LXC on any other linux distibutions through their offical repositories.

In your ubuntu machine install lxc and lxc-templates by putting in terminal
$sudo apt install lxc lxc-templates

Once you have install you can check your default configuration by
$sudo lxc-checkconfig
Now since we have LXC installed on our ubuntu system we can now create our first container
$sudo lxc-create -n mycontainer -t ubuntu
This will download a ubuntu container from internet and save it in your storage with name mycontainer. you need to remember -n option as it will be used everywhere to put your container's name. -t defines the template we want to use. There are many other distribution template available for lxc.You can find them at /usr/share/lxc/templates.
Fig: lxc_template
You can see all available container you have created by using $sudo lxc-ls.
Creating a container doen't mean it is running. To run the container you need to use lxc-start tool.
$lxc-start -n mycontainer
this will start your first container in background. You can check all the running containers by using lxc-ls also.
Fig lxc-ls
this will show the state of all your containers with little more specs. or you can also use lxc-top to list the running container with resources it use.
Fig lxc-top

Now to start using your container all you need to do is attach to the container.To get directly access the container as root user without authentication you can use lxc-attach
$sudo lxc-attach -n mycontainer
this is always not recommended due to root login, so in place you can use lxc-console
$sudo lxc-console -n mycontainer
this will give you login prompt (default username is ubuntu with ubuntu as password).
To dettach from the container just press Ctrl-a followed by q.

If you are using LXC for development or testing purposes then their is high chances that you came across a senario where you require diffrent architechture system then your host.LXC has templates for almost every cpu arch like i836, powerpc and armhf etc.
to create the continer of different architechture you just need to supply --arch= on lxc-create command.Or you can even view the full list and select from them through this command.
$sudo lxc-create --template download --name mycontainer
You can stop a container using lxc-stop or freeze/pause a running container using lxc-freeze and then lxc-unfreeze to unfreeze it.lxc-destroy will delete the container image.To use these tools all you need to do is paas -n containername with these commands.
