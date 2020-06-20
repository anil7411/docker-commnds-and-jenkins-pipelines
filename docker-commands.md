# Docker all commands

##### Used to download base image from docker public registry
```sh
docker pull image:tag  
docker pull ubuntu:latest
```
##### list the images in local server
 ```sh
 docker images
```
##### List the Running containers
 ```sh
 docker ps
```
##### Start and run the new container in detached mode (back ground)
```sh
docker run -dt IMAGE /bin/bash 
```
```sh
docker run -dt ubuntu:latest /bin/bash
```
##### Enter into container environment 
```sh
docker exec -it CONTAINER /bin/bash
````
##### Exit from the container environment
```sh
keyBoard shortcut: ctrl + p + q  (or) exit
```
##### Create a customized images from container
```sh
docker commit CONTAINER nameimage
```
```sh
docker commit bbe1613d50e7  sample:latest
```
##### Tag image to dockerhub repo image
```sh
docker tag <image_in_local>:<tag> <imagehub>:<tag>
```
```sh
docker tag sample:latest anil7411/project:v1.0
```
##### Login to dockerhub
```sh
docker login -u <username> -p <password>
```
```sh
docker login -u anil7411 -p <password>
```
##### Push image to dockerhub
```sh
docker push <imageName>:<tag>
```
```sh
docker push anil7411/project:v1.0
```
##### Download image from dockerhub
```sh
docker pull <imageName>:<tag> 
```
```sh
docker pull anil7411/project:v1.0
```
##### Create new container from image but it wont start the container
```sh
docker create --name <name> <imageName>:<tag>
```
```sh
docker create --name data_container ubuntu /bin/bash
```
##### Start the stopped container 
```sh
docker start <CONTAINER> 
```
```sh
docker start data_container
```
##### Create new container from a image and start that creates container
```sh
docker run -dt --name <name> <imageName>:<tag> /bin/bash
```
```sh
docker run -dt --name ramakrishna 4e5021d210f6 bash
```
##### Create and start new container in interactive mode
```sh
docker run -it --name <name> <imageName>:<tag> /bin/bash
```
```sh
docker run -dt --name ramakrishna 4e5021d210f6 bash
```
##### Enter into running container’s environment
```sh
docker exec -it <CONTAINER> /bin/bash
```
```sh
docker exec -it 2b63f0da4b9e bash
```
##### Run an command in running container from host machine
```sh
docker exec <CONTAINER> <command>
```
```sh
docker exec sample-cont ls (list the files and folders of container)
docker exec  sample-cont mkdir project (create the project directory inside container)
```
##### Copy the files/folder from host machine to container and vice versa
```sh
docker cp <file/folderinhost> <CONTAINER>:/path/to/copy 
```
```sh
docker cp file.txt 73c774879228:/opt (copy to container from host)
docker cp 73c774879228:/opt/container_folder /root(copy  to host from container)
```
##### Export a container's filesystem as a tar archive
```sh
docker export -o <name.tar> <CONTAINER>
```
```sh
docker export -o container.tar cont1
```
##### Import the contents from a tar file to create a filesystem image
```sh
docker import <tarfilename> <nameimage>:<tag>
```
```sh
docker import container.tar customizedimage:latest
```
##### Detail information about container
```sh
docker inspect <CONTAINER>
```
```sh
docker inspect <container_name>
```
##### Save one or more images to a tar archive file
```sh
docker save -o <name.tar> <image1>:<tag> <image2>:<tag> ……….
```
```sh
docker save -o images.tar 8326be82abe6 ed21b7a8aee9 4e5021
```
##### Load an image from a tar archive
```sh
docker load -i <tarfile>
```
```sh
docker load -i images.tar
```
##### see the logs of the container
```sh
docker logs <CONTAINER>
```
```sh
docker logs <container_name>
```
##### List port mappings or a specific mapping for the container
```sh
docker port <CONTAINER>
```
```sh
docker port data_contianer
```
##### Change name of conainer
```sh
docker rename <CONTAINER> <NewName>
```
```sh
docker rename nifty_engelbart cont1
```
##### Restart a container
```sh
docker restart <CONTAINER> 
```
```sh
docker restart cont1
```
##### Stop container
```sh
docker stop <CONTAINER>
```
```sh
docker stop cont1
```
##### Remove container
```sh
docker rm <CONTAINER>
ex:-
```
```sh
docker rm <container_name>
docker rm $(docker ps -aq) (it wiil remove the all container)
```
##### Remove images
```sh
docker rmi <IMAGE> 
```
```sh
docker rmi <image_name>
docker rmi $(docker images -q) (it will remove all images)
```
##### Search the Docker Hub for images
```sh
docker search TERM
```
```sh
docker search ubntu
```
##### Display the running processes of a container
```sh
docker top CONTAINER
```
```sh
docker  top 938e474228b3
```
##### Inspect changes to files or directories on a container’s filesystem
```sh
docker diff CONTAINER
[A->file/folder was added; 
C->file/folder was changed;
D->file/folder wa deleted;]
```
```sh
docker diff 1fdfd1f54c1b
```
##### Display a live stream of container(s) resource usage statistics
```sh
docker stats CONTAINER
```
```sh
docker stats (list the container resources)
docker stats Container (list resources of specific container)
```
##### Remove all stopped containers
```sh
Docker prune  
```
##### Update configuration of one or more containers
```sh
docker update [OPTIONS] CONTAINER
```
```sh
docker update --cpu-shares 512 abebf7571666
docker update --cpu-shares 512 -m 300M abef7571666 hopeful_morse
docker run -dit --name test2 --memory 300M ubuntu bash
```

# DOCKER VOLUMES COMMANDS:-

##### To create named volume, execute the following command:
```sh
docker volume create <name>
```
```sh
docker volume create data
```
##### To list volumes
```sh
$ docker volume ls
```
##### To remove volume,
```sh
docker volume rm <vol_name>
```
```sh
docker volume rm data
```
##### Remove all unused volumes
```sh
$ docker volume prune
```
##### Display detailed information on one or more volumes
```sh
docker volume inspect <vol_name>
```
```sh
docker volume inspect data
```
##### Mount anonimous volume to container 
```sh
$ docker run -dt --name web -v /data ubuntu /bin/bash 
```
##### Mount named volume to container 
```sh
$ docker run -it --name container1 -v myvolume:/datavol ubuntu /bin/bash
     (here myvolume is named volume created by using command docker volume create myvolume)
```
##### Mount host directory to container folder 
```sh
$ docker run -it --name container1 -v /opt:/datavol ubuntu /bin/bash
```
##### Share data between containers
```sh
docker run -it --volumes-from container1 --name container2 ubuntu /bin/bash
```
(We have used a new parameters — volumes-from <containername> that we have specified. This tells container2 to mount the volumes that container1 was mounted.)
```

# DOCKER NETWORK COMMANDS:

##### List the Networks
```sh
$ docker network ls
```
##### To create user defined Bridge network
```sh
Usage:  docker network create [OPTIONS] NETWORK
```
```sh
docker network create -d bridge myBridgeNework
   -d = driver (which network driver should we select)
  If you want to create overlay network then -d “overlay”
```
##### To create user defined Bridge network
```sh
$ docker network create -d overlay myOverlayNework
```
##### Attach container to created network
```sh
$ docker run -it --name cont_name --network=myBridgeNetwork ubuntu 
```
##### Remove unused networks
```sh
$ docker network prune
```
##### Display detailed information on one or more networks
```sh
Usage:  docker network inspect NETWORK
```
```sh
docker network inspect mybridgeNetwork 
```
##### Disconnect a container from a network
```sh
Usage:  docker network disconnect [OPTIONS] NETWORK CONTAINER
```
```sh
docker network disconnect mybridgeNetwork cont_name
```
##### Connect a container to a network
```sh
Usage:  docker network connect [OPTIONS] NETWORK CONTAINER
```
```sh
docker network connect mybridgeNetwork cont_name
```
##### Want to create network with own subnet and gateway
```sh
docker network create -d bridge --subnet 172.17.1.0/24 --gateway 172.17.1.100 mybridge
```

# DOCKER_COMPOSE COMMANDS:

##### Create and start containers
```sh
$ docker-compose up
```
##### Stop and remove containers, networks, images, and volumes
```sh
$ docker-compose down
```
#####  Validate and view the Compose file
```sh
$ docker-compose config
```
#####  Create services
```sh
$ docker-compose create
```
##### Start services
```sh
$ docker-compose start
```
##### Stop services
```sh
$ docker-compose stop
```
##### Display the running processes
```sh
$ docker-compose top
```
#####  List containers
```sh
$ docker-compose ps
```
##### Check logs of containers 
```sh
$ docker-compose logs
```
##### List images
```sh
$ docker-compose images
```
##### Create and start container defined in compose file with different name 
```sh
$ docker-compose -f path/to/file up
```
