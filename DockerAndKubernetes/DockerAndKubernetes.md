## Section01

### 01. Why Use Docker?

Docker makes it really easy to install and run software without worring about setup or dependencies.

### 02. What is Docker?

#### Docker Ecosystem

- Docker **Client**
- Docker **Server**
- Docker **Machine**
- Docker **Images**
  - image : Single file with all the dependencies and configuration required to run a program
- Docker **Hub**
- Docker **Compose**

= Docker is a platform or ecosystem around creating and running containers

- Docker **Container**
  - Instance of an image, Runs a program. 
  - **A program with its own isolated set of HW resources**

### 03. Docker for Mac/Windows

[ Docker Client (Docker CLI) ➜ Docker Server (Docker Daemon) ]

- Docker Client: Tool that we are going to issue commands to
- Docker Server: Tool that is responsible for creating images, running containers, etc

### 04. Installing Docker on MacOS

1. Sign up for a Docker Hub account (docker.com)
2. Download/Install Docker for Mac
3. Login to Docker
4. Verify Docker installation (`docker version`)

### 05. Installing Docker on Windows

`...`

### 06. More Windows Setup

`...`

### 07. One Last Piece of Windows Setup

`...`

### 08. Using the Docker Client

`docker run hello-world`

1. `Docker CLI` 에서 hello-world 실행
2. `Docker Server` 가  `Image Cache` 를 확인
3. `Image Cache` 에 Image가 없을 경우, `Docker Hub` 에서 Image를 가져와서 `Image Cache` 에 저장
4. ` Docker Server` 가 Container를 생성하여 프로그램 실행

### 09.  But Really... What's a Container?

- How OS Runs on Computer ?
  - `kernel`: Intermediate layer that governs access between programs(application) and physical HW
  - `programs (App)` ➜ `System Call` ➜ `Kernel` ➜ `pyhsical HW (CPU, Memory, Hard Disk..)`
  - (example story)  두 개의 다른 응용프로그램이 서로 다른 버전의  python이 필요한 경우,  Hard Disk에 segment를 생성해서 각각 필요한 자원에 접근하도록 함
  - Name spacing: Isolating resources per process (or group of processes), 각 프로세스 마다 필요한 자원을 사용할 수 있도록 하드웨어의 특정 공간을 할당하는 것
  - Control Groups (cgroups): Limit amount of resources used per process
- Then, how about <u>Container</u>?
  - 하나의 프로세스가 컴퓨터에서 작동하기 위한 vertical logic (App to HW, name spacing과 cgroups 포함)의 한 부분
- And, what <u>Image</u> contains?
  - FS Snapshot (copy paste of a very specific set of directories or files)
  - Startup Command

### 10. How's Docker Running on Your Computer?

Name spacing, Cgroups are not included in by default with all OS.  Only in **LINUX**.

So, Linux VM is running on your computer :)

## Section02

### 11. Docker Run in Detail

- Command line format
  - ex) `docker run redis`
  - docker \<command> \<img name>
  - `docker` means Reference the Docker Client

### 12. Overriding Default Commands

`docker run hello-world` `command`

docker 명령어 뒤에 일반 command를 사용하면 container의 default command에 override 되어서 container가 실행될 때command가 실행된다. 단, command가 container fs에 존재할 경우에만 해당한다.

### 13. Listing Running Containers

`docker ps`: listing all running containers

`docker ps --all`: listing all executed containers

- output format
  - \<Container ID> \<Image> \<Command> \<Created> \<Status> \<Posts> \<Names>
  - \<Command>: default command
  - \<Ports>: port for outside access
  - \<Names>: randomly generated name to identify container

### 14. Container Lifecycle

`docker run` = `docker create` + `docker start`

- `docker create <image name>`
  - create a fs (container)
  - print out container id
- `docker start <container id>`
  - start up command
  - `-a <container id>`: means print out the output coming from it
  - docker run has `-a` option as default

### 15. Restarting Stopped Containers

Container stopped doesn't mean that it is dead or cannot be used again.

Can re-start again by docker id, but can't re-override default command because it means trying to start up multiple containers at the same time.

- FS Snapshot is run in the HW (under the Kernel)
- Startup Command is running process (over the Kernel)

### 16. Removing Stopped Containers

`docker system prune`: remove ...

- all stopped containers
- all networks not used by at least one container
- all dangling images
- all build cache (means delete the images from docker hub)

### 17. Retrieving Log Outputs

`docker logs <container id>`: print out the logs of a container

### 18.  Stopping Containers

`docker stop <container id>`: Send <u>SIGTERM</u> to container, shutting container down. Save the container's data.

`docker kill <container id>`: Send <u>SIGKILL</u> to container, shutting container down immediately and doing no additional work.

If container does not automatically stop in 10 secs by `stop`, dockers going to automatically fall back `kill` command. 

### 19. Multi-Command Containers

ex) redis의 경우 redis-server & redis-cli 가 통신하기 위해서는 두개가 같은 콘테이너 안에 존재해야 함. 즉 두개 다른 역할을 하는 something(Image? Container?)이 하나의 콘테이너에 생성되어야 하는 것

### 20. Executing Commands in Running Containers

`docker exec -it <container id> <command>`

- `exec`: Run another command
- `-it`: Allow us to provide input to the container

ex) `docker exec -it <container id> redis-cli`: running container 내부에 redis-cli에 실행함

### 21. The Purpose of the IT Flag

Every container is running inside of virtual machine running Linux.

Every process has three communication channels, STDIN, STDOUT, and STDERR (btween container and terminal).

- `i`: interactive, keep STDIN open even if not attatched

- `t`: --tty, allocated a pseudo-TTY, make interface more prettier

### 22. Getting a Command Prompt in a Container

Overriding default command of container

### 23. Starting with a Shell

`docker run -it <image name> sh`

해당 container 에서 shell을 사용할 수 있음

### 24. Container Isolation

Container는 독립적으로 존재하기 떄문에 자동으로 서로의 fs를 공유하지 않음



## Section03

### 25. Creating Docker Images

build image and run container using Dockerfile

- Dockerfile: plain text file that configure how the container behaves.
- Dockerfile - Docker Client(= Docker CLI) - Docker Server - Usable Image
- Creating Dockerfile
  - Specify a base image
  - Run some commands to install additional programs
  - Specify a command to run on container startup

### 26. Building a Dockerfile

Goal: Create an image that runs redis-server

- `apk` means Apache package
- `docker build .`: 현재 위치(.)의 Dockerfile을 실행함

### 27. Dockerfile Teardown

- docker file format
  - `[instruction][argument]`
  - `instruction`: telling docker server what to do
- `FROM`: docker image to use as a base
- `RUN`: execute while preparing image
- `CMD`: execute when start up a new container 

### 28. What's a Base Image?

- Why use alpine as a base image?
  - evey OS has preinstalled set of programs that are useful
  - apline includes a default set of programs, and it is the why base image is needed

### 29. The Build Process in Detail

- `docker build .`
  - `build`: run Dockerfile and generate image
  - `.`: build context, the set of files and folders that belong to project
- `FROM [image]`: get docker image from Docker Hub
- `RUN`: run command in the image from upper step's image
- create new image that added configuration in every steps, and the last image is the result container

### 30. A Breif Recap

1. Create container
2. Modified FS
3. Take snapshot of that container's FS for next step, or Output the image generated from previous step if no more steps.

### 31. Rebuilds with Cache

If modify the Dockerfile..

- Using cache: docker know nothing has changed from the last time docker built, so use the image that generated during the previous step again.
- If there are no change when running Dockerfile, it will extremely fast :)
- the order change also affected, so can't using cache

### 32. Tagging an Image

- `docker run [image id]`: it is too difficult to remember!
- so, tagging an image!
  - `docker build -t [<Docker Id>/<Name of Repo/Prj>:<version>] .`
  - `docker build -t syaring/redis:latest .`
  - `docker run syaring/redis:latest`

### 33. Manual Image Generation with Docker Commit

- Create Container by Image, and aslo generate Image using by Container