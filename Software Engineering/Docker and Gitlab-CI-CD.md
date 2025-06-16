docker = software platform that simplifies the process of building running and managing applications
virtualises the OS of the computer on which it's installed, enabling the efficient deploument of containerised applications

host multiple web apps on a single server
different frameworks (node.js, spring, flask) different versions of the frameworks, and different dependencies used by each application
Inability to have multiple versions of node.js, java, python ecc... installed on the same machine

without docker: 
- host on multiple physical machines
- hosting on single machine with multiple virtual machines
- high costs associated with procuring and maintaining hardware

with docker:
- Docker Host and Containers
- Logical entities created by Docker Host
- Virtual copy of process table, network interface, and file system mount points
- no OS in contaners
- Kernel of host OS shared across all containers

![[Screenshot 2025-06-16 at 6.04.15 PM.png]]

Docker Networking:

- Bridge network: Default network type, containers on the same bridge network can communicate with each other using container names
- Host Network: container shares the host's network stack directly. Useful for performance but less isolation
- Overlay Network: Enables container communication across multiple Docker hosts, mainly in Docker Swarm mode

PROs
- containers share kernel of the host OS
- multiple containers with different requirements can run on the same host
- Eliminates the need for multiple physical or virtual machines
- Docker allows multiple applications with different requirements to be hosted on the same host
- Containers are generally small and consume very little disk space
- Containers are more robust and boot up quickly compared to VM
- Docker is less demanding on hardware, reducing costs for users

CONs
- Decreased performance due to resource sharing
- Learning curve for new Users
- Limited support for some OSs
- Docker cannot host apps with different OS requirements on the same host
- Apps with different OS requirements must be hosted on separate Docker Hosts

Docker Engine: critical part of the Docker System. manages the Docker platform's overall operations

3 components:
- the Server --> primary module of Docker Engine, operates a daemon named dockerd. Dockerd handles creating and managing Docker Images, Containers, Networks and Volumes
- REST API --> enables applications to interact with the server and order it to execute their functions
- Client --> command-line interface that lets users communicate with docker. allows users to use commands to communicate with the engine
![[Screenshot 2025-06-16 at 6.10.28 PM.png]]

Docker Image = template containing the application and all dependencies required to run that app on Docker. it serves as blueprint for Docker containers. **Images are static**, they are snapshots of an application and its environment

Docker Container = logical entity, essentially a running instance of a Docker Image. Containers are dynamic, they are actual **running instances of images**, executing the application in a Docker environment

 Building Docker Images:
 - each instruction in the building file of the docker image creates a layer
 - these layers are ReadOnly
 - at the end of the build, all R/O layers are stacked and stitched together to form the image
 - this process is knowns as the Union File system

![[Screenshot 2025-06-16 at 6.18.30 PM.png|400]]

image layers are immutable
Union File system allows for layers to be reused when unchanged and recreated when edited

Creating Docker Containers
- made by creating a Read-Write layer on top of a Docker image
- this layer is called the container layer
- Copy-On-Write is the process of copying and editing objects on this layer
- all changes to the container are stored on this layer
![[Screenshot 2025-06-16 at 6.35.08 PM.png]]

Container layer stays on top of the image layers
changes made on the container don't affect the image layers
container layer is removed when docker container is destroyed, resulting in the removal of all changes made to it
![[Screenshot 2025-06-16 at 6.49.00 PM.png|400]]

Store data:
- Volumes --> stored in host filesystems and managed by Docker
- Bind mounts --> stored in host filesystem and need a path
- TMPFS mounts --> availably only on linux and not persistent on the filesystem

data should be isolated from a container to leverage the benefits of adopting containerisation

multi-stage builds in Dockerfile:
- optimizes docker images by separating build and runtime stages
- reduces final image size

create Docker container
```
docker build -t my-app-image
```

run the Node.js app inside the container, expose it on port 3000 and mount the specified local folder `/path/to/local/folder` to the container under the /app directory

```
docker run -d -p 3000:3000 -v /path/to/local/folder:/app my-app-image
```

to push to a registry --> use tag to refer to an image
#### Pushing a Docker Image
![[Screenshot 2025-06-16 at 6.57.07 PM.png]]
#### Pulling a Docker Image
![[Screenshot 2025-06-16 at 6.57.39 PM.png]]

#### Docker Compose
used to defined and manage multi-container Docker apps
can be defined both in yaml and JSON format

The docker-compose.yml file is a configuration file that defines the following:
- services
- networks
- volumes

specifies the configuration for each service, including:
- image to be used
- ports to expose
- environment variables

YAML file: huma-readable data serialization format often used for configuration files and storing structured data.
sensitive to indentation, more error-prone if not careful
clearer syntax, supports comments, allows anchors and aliases to reuse values, easier to write multi-line strings

Environmental Variables
- key : vlaue pairs injected into a container's runtime environment
- used to configure application behavior without changing code
- define them
	- directly in dockefile using `ENV`
	- in docker-compose.yml under `environment`
	- using a `.env` file to centralize definitions
- why use them
	- separate config from code
	- reuse values across services
	- support different environments (dev, test, prod)

# Gitlab CI/CD

CI = Continuous Integration
Software development methodology that delivers high-quality software frequently and efficiently
CI continuously integrates code changes into a shared repository followed by automated tests and builds
with CI, devs can detect and fix errors early in the development cycle, reducing the impact of bugs on the final product

CD = Continuous Delivery/Deployment
CD automates the entire software delivery process, from building to testing to deployment to production
With CD, devs can release software to end-users with high-frequency and confidence

CD = fully automated release

CI/CD helps increase speed of software delivery, enhance collaboration among development teams and improve software quality, it also helps to reduce the overall risk associated with software development

CI/CD several stages:
![[Screenshot 2025-06-16 at 7.11.00 PM.png]]

GitLab CI/CD pipelines are configured via YAML files called ".gitlab-ci.yml"

**Artifacts**: Files generated during the pipeline (test reports, binaries) and saved for later stages or downloads
**Cache**: reusable dependencies to speed up future pipeline runs

GIT WORKFLOW
![[Screenshot 2025-06-16 at 7.15.25 PM.png]]

CI Pipelines are triggered by `git push`
CI Jobs can be run on some specific branches only
