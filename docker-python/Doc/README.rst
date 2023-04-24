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


