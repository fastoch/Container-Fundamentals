# Container-internals

"Understanding Container Internals" by Joshua Jebaraj.  
https://www.youtube.com/watch?v=9ivFrXgB2Zg  

---

# Namespaces

Introduced in Linux kernel version 2.6.24 in **2008**, namespaces define the set of resources that a process can use and interact with.  
They partition kernel resources, allowing one specific set of processes to see one specific set of resources.   

Linux namespaces are a crucial feature for process isolation and resource management.  
They allow the creation of **separate environments within a single Linux system**, each with its own view of system resources.  

There are 7 types of namespaces in Linux, each isolating different aspects of the system:
- **UTS** namespace: Isolates hostname and domain name
- **IPC** namespace: Isolates interprocess communication resources
- **PID** namespace: Isolates process IDs
- **NET** namespace: Isolates network interfaces
- **MNT** namespace: Isolates filesystem mount points
- **USER** namespace: Isolates user and group IDs
- **CGROUP** namespace: Isolates the virtual cgroup filesystem

## Important System Calls

3 important system calls are commonly used with namespaces:
- **clone()**: used to create child processes and can be highly parameterized using flags.
  - It's more versatile than the legacy **fork()** call and can also create threads.
- **unshare()**: disables sharing a namespace with the parent process, effectively creating a new namespace without spawning a new process (except for PID namespaces).
- **setns()**: used to attach a process to an existing namespace.
  - It takes a file descriptor of a symbolic link representing a specific namespace and an optional namespace type check parameter.

These system calls are fundamental for creating and managing namespaces, which are essential for containerization, process isolation, and resource management in modern Linux systems.

## UTS namespace

A Linux feature that isolates the **hostname** and domain name system (DNS) **domain** of a process from the host system.  

- Let's create a new UTS namespace from the CLI: `unshare --uts`
- then, let's change the hostname to `demo` in the same terminal window via `hostname demo`
- if we run `hostname`, we'll get `demo`
- now, if we open a new terminal window and check the hostname, we'll see that the hostname hasn't changed there
  
![image](https://github.com/user-attachments/assets/50f3cf07-d49c-41db-8c6c-eb7e40945de4)

- if we run `exit` to exit the namespace, we'll see that the hostname hasn't changed outside of the namespace

![image](https://github.com/user-attachments/assets/f9abe2d9-f04b-4a7e-8d54-2fda02dd9463)

## IPC namespace

**Inter-Process Communication** namespace is a Linux feature that isolates the IPC resources of a process from the host system.  

To create a new IPC namespace: `unshare --ipc`  
Tools like `unshare` can be used to create new IPC namespaces, while `ipcmk` can create IPC resources within a namespace.  

The main purpose of IPC namespaces is to **isolate IPC mechanisms** such as System V IPC objects and POSIX message queues.  
- Each IPC namespace has its own set of System V IPC identifiers and POSIX message queue filesystem
- IPC objects created within a namespace are only visible to processes that are members of the same namespace
- When an IPC namespace is destroyed (when the last process in the namespace terminates), all IPC objects in that namespace are automatically destroyed

System V IPC objects include:
- Message queues
- Semaphores
- Shared memory segments

IPC namespaces provide a powerful way to isolate and manage inter-process communication resources in Linux, contributing to better resource management and security in containerized environments.

## Process namespace

Provides an isolated process tree for each namespace, allowing isolation of processes inside a container.  
A container cannot see the processes of another container, and cannot see the processes of the host machine.  

To create a Process namespace: ``



@13/48
