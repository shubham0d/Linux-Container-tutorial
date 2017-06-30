Linux Containers

Altough Containers get used by people due to extensively popularity of docker by Docker Inc but linux also have their Containers from several years.
These native linux Containers are named LXC(or simply linux container). LXC project has been their since 2008 and is actively being supported by Cannonical Ltd. It is more of set of tools(more technically say API) which gives you interface to create a container using linux namespaces and cgroups(Control Groups).Linux namespaces(mnt,pid,net,ipc,uts, user) is a feature of kernel used for different type of isolation whereas cgroups are used to set limitation over resources(mostly hardware resources) use by any process.

Mount namespace: A process views different mount points other than the original system mount point. It creates a separate file system tree associated with different processes, which restricts them from making changes to the root file system.
PID namespace: PID namespace isolates a process ID from the main PID hierarchy. A process inside a PID namespace can have the same PID as a process outside it, and even inside the namespace, you can have different init with PID 1.
UTS namespace: In the UTS (UNIX Timesharing System) namespace, a process can have a different set of domain names and host names than the main system. It uses sethostname() and setdomainname() to do that.
IPC namespace: This is used for inter-process communication resources isolation and POSIX message queues.
User namespace: This isolates user and group IDs inside a namespace, which is allowed to have the same UID or GID in the namespace as in the host machine. In your system, unprivileged processes can create user namespaces in which they have full privileges.
Network namespace: Inside this namespace, processes can have different network stacks, i.e., different network devices, IP addresses, routing tables, etc.

LXC is a package that make it possible to use these feature and create something that called as a container.Containers are nothing but a very lightweight VMs with less amount of isolation.Less isolation cause in container due to absent of hypervisor layer, which make container to use the same kernel of host system. But this absence also make container's more widely useble as the overhead of configuring hypervisor has gone and now you can select which resources you wish to isolate according to your usecase.So lets dive into LXC.

