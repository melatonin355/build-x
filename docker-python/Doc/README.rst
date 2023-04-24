Mock Docker 
==================

Resources
==================

-  `Python: A workshop on Linux containers: Rebuild Docker from Scratch <https://github.com/Fewbytes/rubber-docker>`_

- `Python: A proof-of-concept imitation of Docker, written in 100% Python <https://github.com/tonybaloney/mocker>`_

.. code-block:: rst

   +------------------------------------+
   |           Docker Host              |
   |                                    |
   |  +----------------+ +----------+  |
   |  |  Networking    | |  Storage |  |
   |  |    (CNM)       | | (Graph)  |  |
   |  +----+------+----+ +-----+----+  |
   |       |      |            |        |
   |  +----v------+----+ +-----v----+   |
   |  |   libnetwork   | |  Storage |   |
   |  |                | |  Driver  |   |
   |  +----------------+ +----------+  |
   |                                    |
   |  +----------------------------+   |
   |  |         Container           |   |
   |  |                            |   |
   |  |  +--------+ +--------+     |   |
   |  |  | cgroup | | Namespes |     |   |
   |  |  +--------+ +--------+     |   |
   |  +----------------------------+   |
   +------------------------------------+




- **Docker Host:** The machine (physical or virtual) on which the Docker daemon runs.

- **Networking (CNM):** Container Network Model (CNM) is a Docker networking abstraction layer that manages the networking between containers and the host system.

- **Storage (Graph):** The storage layer in Docker that manages images and container layers. This layer uses graph drivers to manage these layers.

- **libnetwork:** A library that handles network management for Docker. It provides the network plumbing for containers, allowing them to communicate with each other and with the host system.

- **Storage Driver:** A plugin-based storage management system for Docker images and container layers. It abstracts the underlying storage technology, allowing for multiple storage backends.

- **Container:** A lightweight, portable, and self-sufficient software unit that runs an application and its dependencies. It shares the host's kernel but has its own isolated file system, networking, and process space.

-  **cgroups:** Control Groups (cgroups) are a Linux kernel feature that allows for the isolation and management of system resources (CPU, memory, disk I/O, etc.) used by processes or containers.

- **Namespaces:** Linux kernel feature that provides isolation between processes and containers by creating a separate view of the system (e.g., process ID, network, mount points, etc.).


Fork and Exec
==================
In the context of Docker and containerization, the fork and exec model is used to create new processes and run commands within a container. The fork and exec model is a standard UNIX process management technique that involves two system calls: fork() and one of the exec family of functions (e.g., execv()).

1. **fork():** This system call is used to create a new process by duplicating the current process. The new process is called the child process, and the original process is called the parent process. Both processes will continue to execute the same code from the point of the fork() call, but with different process IDs (PIDs). The child process typically inherits most of the parent's resources, such as file descriptors and environment variables.

2. **exec:** The exec family of functions is used to replace the current process's memory image with a new program. The exec function call takes a file path and an argument list as parameters. The specified program is then loaded into memory, and the current process's memory is overwritten with the new program. The new program starts executing at its main function, and the original process ceases to exist.

In the context of Docker, these two steps are used to create and manage container processes:

When a Docker container is started, the Docker engine creates a new process (the container process) by forking itself. This new process will have its own process namespace and will be isolated from other processes on the host system.

After creating the child process, Docker uses the exec family of functions to replace the child process's memory image with the specified container image's entrypoint. This causes the container process to execute the entrypoint command (e.g., /bin/sh, /bin/bash, or any other specified command) inside the container environment.

The combination of fork() and exec allows Docker to create isolated container processes, each running a specific command within the container's environment, while still managing and tracking the containers from the parent Docker engine process.


Chroot
==================
`chroot` is a UNIX operation that changes the apparent root directory for the current running process and its children. The term `chroot` stands for "change root." It provides a way to create an isolated environment in which a process and its children can only access a specific part of the filesystem, limited by the new root directory.

In the context of Linux and Docker, `chroot` is used to create a filesystem isolation for a container. When a container is created, the container's filesystem is based on an image, which contains all the files, directories, and dependencies required for the applications running inside the container. By using `chroot`, Docker can restrict the container process to only access its own filesystem, preventing it from accessing or modifying files outside the container.

Here's how `chroot` is used in Docker:

1. When a Docker container is started, the Docker engine sets up the container's filesystem by overlaying the container's image layers on top of each other and mounting the resulting filesystem at a specific location on the host system (e.g., `/var/lib/docker/overlay2/<container-id>/merged`).

2. Docker then creates a new process (usually using the fork and exec model) that will run inside the container.

3. Before executing the container's entrypoint command, Docker calls the `chroot()` system call to change the root directory of the container process to the container's mounted filesystem. This restricts the container process and its children to only access files and directories within the container's filesystem.

4. The container process now runs in an isolated filesystem environment, separate from the host system and other containers.

It's important to note that `chroot` only provides filesystem isolation, and it doesn't offer complete isolation for other system resources, such as process IDs, network interfaces, and IPC namespaces. Docker uses other Linux kernel features like namespaces and cgroups to provide a more comprehensive isolation for containers.
