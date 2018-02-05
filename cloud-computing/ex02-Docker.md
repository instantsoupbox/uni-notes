# Exercise 2 Docker Installation & Configuration

## Concepts
* Docker: Open source project to automate application deployment inside containers. 
* Avoids the overhead of starting and maintaining VMs by allowing independent containers to run within a single Linux instance.
* Properties:
    * Lightweight
    * Resource isolation
    * Separate namespaces
    * Isolates the application's view of the OS - almost private view of the OS.
* Advantages:
    * Simplification of creation of highly distributed systems.
    * Multiple applications may run autonomously.
    * Enables on-demand deployment.
    * PaaS-style of deployment and scaling.
* Typically organizations run 5 containers per host.

## Docker Engine
* The layer on which Docker runs.
* A client-server application consisting of 
    * **Docker daemon** (server) running on the host machine (`$ dockerd`)
    * **Docker CLI** (client) not necessarily running on host machine, remote communication with docker daemon possible (`$ docker`)
    * **REST API** to interact with the Docker daemon remotely.

## Dockerfile
* Contains instructions on how to build an image.

## Image
* A template with instructions on how to create a container. 
* Describes the filesystem and parameters to use at runtime.
* Does not have a state and never changes.

## Container
* Running instance of an image.
* Defined by its image.

