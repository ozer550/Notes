![[problems with server hosting.png]]

## Problem with Tradiational Servers
- Consider the following scenario here we have 2 apps requiring same dependencies libfoov1 (green and blue) therefore we can install this dependecy and both the apps would work fine simultaneously.
- Now a third application comes(red) which requires libfoov2 specifically. now upgrading the libfoov1 to v2 would break green and blue app. Due to this we cant run all the three apps together as the depnedencies for all three are conflicting and will cause the other one to break. 
- Once the server collapses there is no mechanism for disaster recovery here. The product would be unaccesible!
- Poor Resiliency

## Containers
- A container is a running instance of a container image.
- A container image contains information about the resources needed by your application including the app itself.
- It contains information about Application and everything that is required by this application.
- Image also has meta data like entry point which specifies what commands get triggered after the container is started from image.

## Vitual Machines vs Containers

![[vm vs containers.png]]

## Benefits of Containers
- Small footprint (docker image files are comapretively smaller in size and contains lots of metadata and information)
- Only resources needed by the application are baked into the container.
- Reduces time to market and cost.
- Portability due to no fully feldged operating sytem running in.
- Scalabilty and resiliency.
![[containerization startup.png]]
## Linux Features that Support 

1. Namespaces
	Namespaces in Linux are a feature that allows multiple instances of system resources, such as network interfaces or process IDs, to exist on the same system without interfering with each other. This helps to isolate different parts of the system and ensures that one part of the system doesn't affect others.
	
2.  Control groups
	cgroups, or control groups, are a feature in Linux that allow you to limit and control the resources (such as CPU, memory, and network bandwidth) used by a group of processes. This helps you manage and distribute system resources in an efficient and controlled manner.
	
	Think of cgroups as a budget for resources in Linux. Just like you might budget your money to ensure you don't overspend, cgroups allow you to budget the resources used by processes, such as how much memory they can use or how much CPU time they are allowed. This ensures that one process or group of processes doesn't consume all of the available resources, leaving none for other processes.
3. SE-Linux
	SELinux (Security-Enhanced Linux) is a security feature in Linux that provides mandatory access control (MAC) for processes and files. It works by defining and enforcing security policies, which determine what actions a process is allowed to take and what files it can access.
