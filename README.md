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
- **PID** namespace: Isolates process IDs
- **NET** namespace: Isolates network interfaces
- **MNT** namespace: Isolates filesystem mount points
- **UTS** namespace: Isolates hostname and domain name
- **IPC** namespace: Isolates interprocess communication resources
- **USER** namespace: Isolates user and group IDs
- **CGROUP** namespace: Isolates the virtual cgroup filesystem

## Important System Calls

3 important system calls are commonly used with namespaces:
- **clone()**: This system call is used to create child processes and can be highly parameterized using flags. It's more versatile than the legacy fork() call and can also create threads.
- **unshare()**: This system call disables sharing a namespace with the parent process, effectively creating a new namespace without spawning a new process (except for PID namespaces).
- **setns()**: This system call is used to attach a process to an existing namespace. It takes a file descriptor of a symbolic link representing a specific namespace and an optional namespace type check parameter.


@6/48
