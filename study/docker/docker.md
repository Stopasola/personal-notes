


## What is a container ?

- A way to package application with all the necessary dependencies and configurations.
- It is portable an easily shared, making the process to develop and deploy more efficient.
- All containers are composed by layers, they can be manipulated in granular  way, allowing updates only in specific layers and is possible more than one container reuse a common layer. 

## Where container are stored ??

-  Some companies have their own repositories, but mainly is possible to find almost any container on [DockerHub](https://hub.docker.com/).

## Docker network

?????



## Before docker

All developers was obligated to install every dependencies on their local machine, causing a bunch of problems. 

## Docker != Image

An **image** can be defined as the actual package that can be moved arround, the **container** is the image after being created.
Resuming:

#### Container  
 
  -  is a running enviroment for ***image***. 
  - every container has a ***port binded*** responsable to talk with the application running inside.
  -  Containers has no data persistencem, when one container is removed, all data updated when the container was up, is gone.   

#### Image
  - Can be defined basically as the appplication stored in a file 
  - Not Running => Image

#### Docker vs Virtual Machine

Both are virtualization tools, the difference is that  docker only virtualizes the applications layer of the OS. The Virtual Machine virtualize the Applications layer AND the Kernel layer. We can see an difference in time and size of each, considering that docker does not virtualize an extra layer. But the Virtual Machine can be runned in all OS, and with docker it is needed to install  a tool called Docker Toolbox.

## Bind ports 

Docker containers can connect to the outside world without further configuration, but the outside world cannot connect to Docker containers by default. Suppose you want to expose the container’s port 8080 (standard http port) on the host’s port 80 (assuming that port is not in use). Append `-p 80:8080` (or --publish=80:8080) to your docker run command. 

Example: `docker run -p 80:8080 nginx`

## Developing process with docker

Here, the course will show a real case scenario of how create an application and use it with docker.

#### Dockerfile Commands and Format

You can see the full documentation [_here_](https://docs.docker.com/engine/reference/builder/#format), on **docker docs**.

- **FROM**: A `Dockerfile`  **must** begin with a  `FROM`  instruction. The instruction specifies the **Parent Image** from which you are building. `FROM` may only be preceded by one or more `ARG` instructions, which declare arguments that are used in `FROM` lines in the `Dockerfile`.  There is the chance of creating an image from scratch, and basically you will have an blank image.

- **ENV**: Environment variables (`ENV`  statement) can also be used in certain instructions as variables to be interpreted by the `Dockerfile`.
The `ENV` instruction sets the environment variable `<key>` to the value `<value>`. This value will be in the environment for all subsequent instructions in the build stage, and can be replaced inline in many as well. The value will be interpreted for other environment variables, so quote characters will be removed if they are not escaped.

- **RUN**:  The `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the `Dockerfile`.  Used to execute commands in the process of mount of the image. A great feature of the `RUN` command is that each layer can be used again in other images, if you create a first image, and then create a second one with one more command `RUN`, during the build of the second, the process will be faster, because all commands common in both images will be reused.

- **CMD**:  This command its almost like `RUN`, but it executes the command only in the **creation** of the container, not in the build of the image. 
There can only be one `CMD` instruction in a `Dockerfile`. If you list more than one `CMD` then only the last `CMD` will take effect.

- **ADD** && **COPY**:  copies new files or directories from `<src>` and adds them to the file system of the container at the path `<dest>`.
The  **ADD** copy remote files and being able to download by URL or copy and then unpack .tar files.


 copia arquivos remotos para alguma pasta na imagem e também pode copiar arquivos compactados (tar.gz) que serão descompactados na imagem automaticamente.
     Eg:		
		     >   
	      


Multiple <src> resources may be specified but the paths of files and directories will be interpreted as relative to the source of the context of the build.



 
## Dockerfile

Docker can build images automatically by reading the instructions from a  `Dockerfile`. A  `Dockerfile`  is a text document that contains all the commands a user could call on the command line to assemble an image. 



## Docker Commands


> `docker start [OPTIONS] <container id>`

- When it is not specified, the docker image downloaded on DockerHub will be the latest.
- 

> `docker run package:version`

The command `run` first search for the selected package instaled on the current machine, if not found, realize the pull from DockerHub](https://hub.docker.com/). FInally the container is started. Docker can run your container in detached mode, that is in the background. To do this, we can use the `--detach` or `-d` for short. Docker will start your container the same as before but this time will “detach” from the container and return you to the terminal prompt. It is possible to, use the command `--name {desired name}` and choose your image name.

> `docker ps`

List all running containers. Can be used with `-a`, and will show all containers (running and not running).

> `docker images`

This command lists all the locally stored docker images.

> `docker run`

This command is used to create a container from an image and will run after. If there is no container installed, this command will execute `docker pull` before


> `docker start `

This command launches a container previously stopped.

> `docker stop {container_id}`

This command stops a running containerl

> `docker logs {container_id}  || docker logs {container_name}`
 
This commands outputs the logs of an specific container.

> `docker exec` 

Ex: `docker exec -it <container_id> bash`

This command is used to access the running container. The bash means the name of the terminal, and the `-id` means interactive terminal. When you go inside, you can see all the container structure, and run linux commands (it is installed some lightweight versions of linux, so some common commands like `curl` wont be available by default).

> `docker rmi`

This command is used to delete an image.

Eg: `docker rmi <image_id>`

## Running multiple commands with  "Docker compose"


- Instead of running commands one by one on terminal, with docker compose you can write in a `.YAML` file in a structured way the same docker commands, but with the power of creating multiple at a time and docker compose take cares  of creating a common network.

Ex: Execute docker-compose file

> docker-compose -f flename.yaml up

Ex:  Stop containers && delete the created network

> docker-compose -f filename.yaml down


## Real Example

Description: App that uses mongo


> docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo

> docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --name mongo-express --net mongo-network -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express
 

Obs: all variables can be foun inside the docker hub of the image, then you can pass this parameters as you want.

 