# Docker all commands

used to download base image from docker public registry
```sh
docker pull image:tag  
docker pull ubuntu:latest
sh
list the images in local server
 docker images

list the Running containers
 docker ps

#start and run the new container in detached mode (back ground)
docker run -dt IMAGE /bin/bash 
ex:
 docker run -dt ubuntu:latest /bin/bash

#Enter into container environment 
docker exec -it CONTAINER /bin/bash

#Exit from the container environment
 ctrl + p + q   
    (or)
 exit

#create a customized images from container
docker commit CONTAINER nameimage
ex:-
 docker commit bbe1613d50e7  sample:latest

#tag image to dockerhub repo image
docker tag <image_in_local>:<tag> <imagehub>:<tag>
ex:-
 docker tag sample:latest anil7411/project:v1.0

#login to dockerhub
docker login -u <username> -p <password>
ex:-
 docker login -u anil7411 -p <password>

#push image to dockerhub
docker push <imageName>:<tag>
ex:-
 docker push anil7411/project:v1.0

#download image from dockerhub
docker pull <imageName>:<tag> 
ex:-
 docker pull anil7411/project:v1.0
  
#create new container from image but it wont start the container
docker create --name <name> <imageName>:<tag>
ex:-
 $ docker create --name data_container ubuntu /bin/bash

#start the stopped container 
docker start <CONTAINER> 
ex:
 $ docker start data_container

#create new container from a image and start that creates container
docker run -dt --name <name> <imageName>:<tag> /bin/bash
ex:
 $ docker run -dt --name ramakrishna 4e5021d210f6 bash

#create and start new container in interactive mode
docker run -it --name <name> <imageName>:<tag> /bin/bash
ex:-
 $ docker run -dt --name ramakrishna 4e5021d210f6 bash

#Access/enter into running container’s environment
docker exec -it <CONTAINER> /bin/bash
ex:-
 $ docker exec -it 2b63f0da4b9e bash

#Run an command in running container from host machine
docker exec <CONTAINER> <command>
ex:-
 docker exec sample-cont ls (list the files and folders of container)
 docker exec  sample-cont mkdir project (create the project directory inside container)

#copy the files/folder from host machine to container and vice versa
docker cp <file/folderinhost> <CONTAINER>:/path/to/copy 
ex:-
 docker cp file.txt 73c774879228:/opt (copy to container from host)
 docker cp 73c774879228:/opt/container_folder /root(copy  to host from container)

# Export a container's filesystem as a tar archive
docker export -o <name.tar> <CONTAINER>
ex:-
 docker export -o container.tar Ramakrishna

#Import the contents from a tar file to create a filesystem image
docker import <tarfilename> <nameimage>:<tag>
ex:-
 docker import container.tar customizedimage:latest

#Detail information about container
docker inspect <CONTAINER>
ex:-
 docker inspect <container_name>

#Save one or more images to a tar archive file
docker save -o <name.tar> <image1>:<tag> <image2>:<tag> ……….
ex:-
 docker save -o images.tar 8326be82abe6 ed21b7a8aee9 4e5021

#Load an image from a tar archive
docker load -i <tarfile>
ex:-
 docker load -i images.tar

#see the logs of the container
docker logs <CONTAINER>
ex:-
 docker logs <container_name>


#List port mappings or a specific mapping for the container
docker port <CONTAINER>
ex:-
 docker port data_contianer

#change name of conainer
docker rename <CONTAINER> <NewName>
ex:-
 docker rename nifty_engelbart Gowtham

#restart a container
docker restart <CONTAINER> 
ex:-
 docker restart Gowtham

#stop container
 docker stop <CONTAINER>
ex:-
 docker stop Krishna

#remove container
docker rm <CONTAINER>
ex:-
 docker rm <container_name>
 docker rm $(docker ps -aq) (it wiil remove the all container)

#remove images
docker rmi <IMAGE> 
ex:-
 docker rmi <image_name>
 docker rmi $(docker images -q) (it will remove all images)

# Search the Docker Hub for images
docker search TERM
ex:-
 docker search ubntu

#Display the running processes of a container
docker top CONTAINER
ex:-
 docker  top 938e474228b3

#Inspect changes to files or directories on a container’s filesystem
docker diff CONTAINER
[A->file/folder was added; 
C->file/folder was changed;
D->file/folder wa deleted;]
ex:-
 $ docker diff 1fdfd1f54c1b

# Display a live stream of container(s) resource usage statistics
docker stats CONTAINER
ex:-
 docker stats (list the container resources)
 docker stats Container (list resources of specific container)

#Remove all stopped containers
Docker prune  

#Update configuration of one or more containers
docker update [OPTIONS] CONTAINER
ex:- 
 $ docker update --cpu-shares 512 abebf7571666
 $ docker update --cpu-shares 512 -m 300M abef7571666 hopeful_morse
 $ docker run -dit --name test2 --memory 300M ubuntu bash

=================================================================================
=================================================================================
=================================================================================
=================================================================================

DOCKER VOLUMES COMMANDS:-
========================
#To create named volume, execute the following command:
 docker volume create <name>
 ex:
$ docker volume create data

#To list volumes
$ docker volume ls

#To remove volume,
docker volume rm <vol_name>
ex:-
$ docker volume rm data

#Remove all unused volumes
$ docker volume prune

#Display detailed information on one or more volumes
docker volume inspect <vol_name>
ex:
$ docker volume inspect data

#mount anonimous volume to container 
$ docker run -dt --name web -v /data ubuntu /bin/bash 

#mount named volume to container 
$ docker run -it --name container1 -v myvolume:/datavol ubuntu /bin/bash
     (here myvolume is named volume created by using command docker volume create myvolume)

#mount host directory to container folder 
$ docker run -it --name container1 -v /opt:/datavol ubuntu /bin/bash

#share data between containers
$ docker run -it --volumes-from container1 --name container2 ubuntu /bin/bash
  (We have used a new parameters — volumes-from <containername> that we have specified. 
   This tells container2 to mount the volumes that container1 mounted.)

================================================================================================================
================================================================================================================
================================================================================================================
================================================================================================================

DOCKE NETWORK COMMANDS:
=======================

Docker Network commands: -
#list the Networks
$ docker network ls

# to create use defined Bridge network
 Usage:  docker network create [OPTIONS] NETWORK
$ docker network create -d bridge myBridgeNework
        -d = driver (which network driver should we select)
    If you want to create overlay network then -d “overlay”
    
# to create user defined Bridge network
$ docker network create -d overlay myOverlayNework

# attach container to created network
$ docker run -it --name cont_name --network=myBridgeNetwork ubuntu 

#remove unused networks
$ docker network prune

#Display detailed information on one or more networks
 Usage:  docker network inspect NETWORK
$ docker network inspect mybridgeNetwork 

#Disconnect a container from a network
Usage:  docker network disconnect [OPTIONS] NETWORK CONTAINER
$ docker network disconnect mybridgeNetwork cont_name

#Connect a container to a network
Usage:  docker network connect [OPTIONS] NETWORK CONTAINER
$ docker network connect mybridgeNetwork cont_name

# want to create network with own subnet and gateway
$ docker network create -d bridge --subnet 172.17.1.0/24 --gateway 172.17.1.100 mybridge

===============================================================================================================
===============================================================================================================
===============================================================================================================
===============================================================================================================

DOCKER_COMPOSE COMMANDS:
=======================

# Create and start containers
  $ docker-compose up

# Stop and remove containers, networks, images, and volumes
$ docker-compose down

# Validate and view the Compose file
$ docker-compose config

# Create services
$ docker-compose create

# Start services
$ docker-compose start

# Stop services
$ docker-compose stop

# Display the running processes
$ docker-compose top

# List containers
$ docker-compose ps

# View output from containers
$ docker-compose logs

# List images
$ docker-compose images

# create and start container defined in compose file with different name 
$ docker-compose -f path/to/file up
