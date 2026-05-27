# 3. Containers & Dockers

## 3.1. General principles

Purpose:

- Way to make applications reproducible

- **Avoid conflicts**: files system that contain all bundled dependencies and own operational system for given application to work

- **OS standardization**: avoid issues with different operational systems, hardwares and software versions 

- **Safe**: host machine files cannot be accessed

- **Ease-of-use**: access, use and upgrade

Container:

- **Portable** package **holding** applications and all its necessary means

- **Lightweight, stand-alone and executable** package of a piece of software

- Includes **everything needed to run it**: code, routine, system, tools, libraries, settings

Docker:

- Open-source platform

- **Manage container** images: load, run and share 

#### Main artefacts

- Container recipe / Dockerfile
  
  - A script you build with all requirements, structure and policies of the containers
  
  - Needs to live in Git environment

- Docker image
  
  - Base to build the container
  
  - Contains all specifications from the dockerfile
  
  - Long-term storage, made for distribution

- Container
  
  - The system to be reproduced itself
  
  - Short-term lived, only during its call

- Backup.tar
  
  - Compacted file of the image **during run**
  
  - Mainly for internal usage, should not be used for reproducibility

- Docker Registry
  
  - Where docker images and version histories are stored 

- Docker Engine
  
  - Who access and manages docker images and containers

## 3.2. Using Docker Images

Docker images should be preferably stored in a Docker Registry. This is where you would also find available Images to use.

#### Get images from registries

To get an image, first assure your Docker Engine is running (in Docker Desktop). The `docker` command refers to the Docker engine commands. To get an image:

```bash
docker pull <image>:<version>
```

If you don't specify `<version>` docker will pull the latest.

To check all the images existing on your machine:

```bash
docker images
```

The symbol `U` means the Image is being used by a running container.

It is possible to copy a image, giving a personalized name, through the `tag` command. The `image_ID` can be checked through `image`.

```bash
docker tag <image_ID> <new_name:new_version>
```

#### Run containers from the image

You can run containers with or without dacker and image arguments. Docker arguments are universal, image arguments are specific to the image - though some of them are the same across multiple images.

```bash
docker run [docker_args] <image> [image_args]
```

If you pull a Linux OS like `Ubuntu`, you can run all bash commands as image arguments, such as `pwd`, `cd`, `ls` ...

To run a container in the background:

```bash
docker run -d <image>    # `-d` or `--detach`
```

#### Managing existing containers

To list running containers and all containers (wheter or not running):

```bash
docker ps
```

```bash
docker ps -a
```

It mught be useful to rename the container when running it, so you don't get lost if you are running more than 1 container from the same image:

```bash
docker run -d --name <my_container_name> <image>
```

To check global usage from Images and Containers:

```bash
docker system df
```

#### Clean up

Images and Containers occupy a lot of disk and RAMspace. There are several ways to remove them. For unused objects:

- Remove unused containers:
  
  ```bash
  docker container prune
  ```

- Remove all unused objects (**use it carefully!!**)
  
  ```bash
  docker system prune -a
  ```

Containers there are running and Images associated with them must be removed individually (also works for non-running objects):

- Container:
  
  ```bash
  docker rm -f <container_name_or_ID>
  ```

- Image:
  
  ```bash
  docker rmi <image_name_or_ID>
  ```

If you copy a Image with the same name, or create a Image with the same name of an existing one from a Dockerfile, you create **Dangling Images**. They cannot be accessed or run Containers from them anymore.

- Remove dangling Images:
  
  ```bash
  docker image prune
  ```

- Remove all dangling files (Images and Containers associated with them):
  
  ```bash
  docker system prune
  ```

#### Access Containers Interactively

You can access a Container interactively, and run multiple commands inside it:

```bash
docker run -it <image> bash
```

To exit the Container type `exit`; this will stop it as well. The `bash` command might not be necessary for some Images, but it will always work so it is better to keep it.

It is best practice to automatically remove the Container after using it (unless there is a specific need for it to be kept in history). The command for that is:

```bash
docker run --rm <image>
```

There might be some internal Container's commands that don't behave usually if directly called. When this is the case, you can try to call:

```bash
docker run <image> bash -c "<command>"
```

## 3.3. Mounting Volumes & Using Ports

Volumes are a way to set a connection with a local directory and a specific folder inside the Container. 

#### Setting volumes

It is important to **set the correct working directory** inside the Container so the commands to be run access the correct files.

To check the working directory set in the Image, you can `pwd` running the Container interactively or with:

```bash
docker inspect <image> | grep "WorkingDir"
```

If nothing appears, it is because root is the working directory. Usually leaving root as the working directory is not a best practice because you might overwrite the Container's system.

To set a different working directory when running the Container:

```bash
docker run -w [/absolute/path/to/working/directory]
```

To mount a volume between a local and a container directory:

```bash
docker run -v [local/path]:[/container/absolute/path] # Or --volume
```

It is not necessary to mount the volume the Container working directory, but it needs to be consistent with the tasks you are running. The local path must be set in one of these ways:

- Relative to working directory: `./relative/path`

- Relative to home: `~/relative/path`

- Absolute: `/home/User/repository/data`

Wrapping up, to run a Container, set its working directory, mount the volume with local  directory and remove it:

```bash
docker run --rm -w [/work/dir] -v [loc/path]:[/work/dir]
```

It might be interesting to first run interactively (`-it`) to see if the volume was correctly created, which can be done by checking the working directory (`pwd`) and and if the files from the local folder appear in the mounted volume (`ls`).

#### Using Ports

Ports are a way to establish a communication with a webserver. Usually Containers that connect to a webserver (such as `nginx`) keep running until they are terminated, that is why they should be run detached. There are two ways of creating ports:

- **Temporary connection**: if you run the Container detached, you can run commands inside it afterwards with `exec`, and to access a server:
  
  ```bash
  docker exec <webserver_container> curl localhost:80
  ```

- **Permanent connection**: to keep the port with the server open while the Container is running, you need to set the local host (or webserver) and the host inside the Container, and access it:
  
  ```bash
  docker run -d --publish <local>:<container> webserver_image
  ```
  
  This is like mounting a volume, but within servers. After running with the port published it is possible to open the server directly:
  
  ```bash
  curl localhost:<local>
  ```

## 3.4. Creating Images

Docker recipes contain all information required to create a Docker Image. The default recipe name is *Dockerfile*. Best practice is that there is only 1 Dockerfile per folder.

A good insight on what the original recipe of a docker Image had can be done by analyzing the output of:

```bash
docker inspect <image>
```

#### Building an Image

To build an Image from a Dockerfile:

```bash
docker build <options> <image-name:version> <path>
```

Path points to where the image will be created. The version is not oblied but it is good practice to define it.

The most common command to build would be in the current directory (`.`):

```bash
docker build -t my_image:last_version .
```

If there is more than 1 Dopckerfile in the directory, then:

```bash
docker build -f Dockerfile.prod -t my_image:last_version .
```

To check the layers of the created image (including the ones from imported images to it), you can check:

```bash
docker history <image-name:version> 
```

#### Writing a Dockerfile

The general specifications of a Dockerfile should be:

- Install applications dependencies

- Specify the container environment

- Set up application directories

- Define runtime configuration

- Provide image metadata

The link below explain most common commands of a Dockerfile: [How to Create Docker Image from Dockerfile | phoenixNAP KB](https://phoenixnap.com/kb/create-docker-images-with-dockerfile)

General guidelines in writing recipes are:

1. Build upon existing images

2. Specify versions for tools, images and softwares

3. Do proper formatting and documentation

4. Keep size as low as possible

5. Keep data outside image/container (mount them at run time)

6. Check licenses

7. Make the container discovarable

#### Pushing Images to a registry

To push images to Dockerhub (or any other registry), you first need to be logged in. Even if you are logged in the Docker desktop app, you should run to authenticate:

```bash
docker login
```

For reproducibility, the image should be tagged. You should include the namespace of your image (your registry username in standard cases), the repository of the image (the name for which it should be found) and its version. To push to a registry other than Dockerhub, it should also be specified.

```bash
docker tag <image> <registry>/<namespace>/<repository>:<version>
```

To push the image:

```bash
docker image push <image-tag>
```

## 3.5. Apptainer Structure

Apptainer is a more flexible alternative to Docker, which allows it to be used inside HPC structures. The table below compares both.

| Docker                                                                         | Apptainer                                                           |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| :white_check_mark: Accessible through Desktop                                  | :x: Only accessible through a shell                                 |
| :white_check_mark: Good support for all OS                                     | :x: Good support for Linux only                                     |
| :white_check_mark: Enforces some reproducibility (Docker controls image files) | :x: Reproducibility is your burden                                  |
| :white_check_mark: You have all permissions inside container                   | :x: Some features might need root (or sudo) permissions             |
| :white_check_mark: Customized script for recipes                               | :x: bash-intensive recipes                                          |
| :x: root permissions might cause disruptions                                   | :white_check_mark: Can be run as a simple user                      |
| :x: Image is completely handled by docker                                      | :white_check_mark: Image is a file (or directory): easier to manage |
| :x: Dependency on Docker to manage your system                                 | :white_check_mark: You manage your Containers structure             |
| :x: Docker running in the background: high RAM usage                           | :white_check_mark: Easily portable                                  |
| :x: Does **not** run in HPC                                                    | :white_check_mark: Runs in HPC                                      |

#### Get Apptainer images

Apptainer images are files. They don't need to have a specific format, but for reproducibility they are usually saved as `my_image.sif`. The Image repository also need to be specifed, either directly or by an URL.

- Pulling from singularity hub:
  
  ```bash
  apptainer pull <image-name>.sif shub://<registry/image> 
  ```

- Pulling from Docker hub:
  
  ```bash
  apptainer pull <image-name>.sif docker://<registry/image:version>
  ```

- Pulling from URL:
  
  ```bash
  apptainer pull <image-name>.sif <https://server.prg/image>
  ```

If there is a doubt wheter a file is an apptainer image or not, it can be checked through:

```bash
file <image>
```

#### Inspect Images

By default there are 2 types of inspections of apptainer images: one as in docker and another one as how the container output should bem which is called by:

```bash
apptainer inspect --runscript <image-name>.sif
```

The `inspect --runscript` is more verbosed in apptainer, when the default `inspect` shows only basic information of the image.

#### Manage Images

Apptainer Images are files, so they are managed by the CLI:

- Move or rename
  
  ```bash
  mv <image>.sif /new/path/<new-name>.sif
  ```

- Check directory usage
  
  ```bash
  tree --du -h -d -shaC
  ```

- Check files usage
  
  ```bash
  ls -lh
  ```

- Clean up
  
  ```bash
  rm -f *.sif
  ```

#### Run apptainer containers

As a first-time run of a container, it is best practice to run it interactively (access system directory structure through bash):

```bash
apptainer shell <image>.sif
```

If not interactively, apptainer always run containers detached. This can be done through `run` in a similar way as in docker, though it can also be done through `exec`. As in docker, only with `exec` it is possible to run commands in a container already running in the background.

```bash
apptainer run <image>.sif <image-command>
```

```bash
apptainer exec <image>.sif <image-command>
```

## 3.6. Mounted Volumes in Apptainer

By default, **volumes are already mounted** when a apptainer container is run. The mounting directory depends on system; in HPC, the user data (`$VSC_HOME`, `$VSC_DATA` and `$VSC_SCRATCH`) are mounted to `/kyukon`, and the remaining folder structure keeps both the HPC and the container folder structure, but without access.

The directory you run the container will always be mounted to the container structure, but it is possible to mount other directories:

```bash
apptainer shell -B /path/in/host:/path/in/container <image>.sif
```

## 3.7. Using Apptainer in HPC
