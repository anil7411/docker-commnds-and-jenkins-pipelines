# Dockerfile examples
 
1. RUN INSTRUCTION:
  The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. In simple way, RUN instruction is used to execute the commands inside the container.
  
```sh
#Create a file with Dockerfile add below commands 
FROM ubuntu:latest
RUN apt update -y 
RUN apt install wget -y 
RUN apt install openjdk-8-jdk -y
```
---------------------------------------------------------------------------------------------------------
```sh
command to create a image from docker file
$ docker build -t sample:image . 
```

2. CMD INSTRUCTION:
CMD instruction allows you to set a default command, which will be executed only when you run container without specifying a command. If Docker container runs with a command, the default command will be ignored. If Dockerfile has more than one CMD instruction, all but last CMD instructions are ignored.

```sh
ex1:-
FROM ubuntu:latest
CMD echo "hello world"
=========================================================================================================
ex2:-
FROM ubuntu:latest
RUN apt update -y && apt install wget -y &&  apt install openjdk-8-jdk -y
RUN wget http://apachemirror.wuchna.com/tomcat/tomcat-8/v8.5.54/bin/apache-tomcat-8.5.54.tar.gz
RUN tar -xvf apache-tomcat-8.5.54.tar.gz
CMD ["bash", "/apache-tomcat-8.5.54/bin/catalina.sh", "run"]
```
3.ENTRYPOINT INSTRUCTION:
ENTRYPOINT instruction allows you to define Command that will be executed only when you run the container. It looks similar to CMD, the difference is ENTRYPOINT command and parameters are not ignored when Docker container runs with command line parameters.

```sh
FROM ubuntu:latest
RUN apt update -y && apt install wget -y &&  apt install openjdk-8-jdk -y
RUN wget http://apachemirror.wuchna.com/tomcat/tomcat-8/v8.5.54/bin/apache-tomcat-8.5.54.tar.gz
RUN tar -xvf apache-tomcat-8.5.54.tar.gz
CMD ["bash", "/apache-tomcat-8.5.54/bin/catalina.sh", "run"]
ENTRYPOINT echo "hello world"
```
4. COPY INSTRUCTION:
The COPY instruction allows you to copies the files/directories from only docker host to container files system. Therefore, you cannot use it with URLs to copy external files to your container.

```sh
CREATE A DOCKERFILE WITH A NAME "dockerfile2"
FROM  ubuntu:latest
COPY file.txt /
```
----------------------------------------------------------------------------------------------------
```sh
TO RUN BUILD A IMAGE FROM above named DOCKEFILE "dockerfile2" use below command

$docker build -t sample:latest -f  /path/to/dockerfile2 . (dont forgot to add '.' at end)
```

5. ADD INSTRUCTION:
The ADD instruction allows you to copies the files/directories from docker host or from URL to container files system.

```sh
ex1:
FROM ubuntu
ADD apache-tomcat-8.5.54.tar.gz /opt

ex2:
FROM ubuntu 
ADD http://apachemirror.wuchna.com/tomcat/tomcat-8/v8.5.54/bin/apache-tomcat-8.5.54.tar.gz /opt

ex3:
FROM ubuntu
ADD file1.txt /tmp
```

6. WORKDIR INSTRUCTION:
The WORKDIR instruction provides a way to set the working directory for the container and the ENTRYPOINT and/or CMD to be executed when a container is launched from the image.  If the WORKDIR doesn’t exist, it will be created even if it’s not used in any subsequent Dockerfile instruction.

```sh
FROM ubuntu
WORKDIR /home/ubuntu
```
7. ENV INSTRUCTION:
The ENV instruction is used to set environment variables those variables can available during the image build process and container running.

```sh
ex1:-
FROM ubuntu
ENV NAME=JAVA
ENV VERSION=1.8

ex2:-
FROM ubuntu
RUN apt update -y && apt install openjdk-8-jdk -y
ADD https://mirrors.estointernet.in/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz /opt
WORKDIR /opt
RUN tar -xvf /opt/apache-maven-3.6.3-bin.tar.gz
ENV MAVEN_HOME=/opt/apache-maven-3.6.3
ENV PATH=$PATH:$MAVEN_HOME/bin
```
8.ARG INSTRUCTION:
ARG Instruction used to set Enviroment variables in the Dockerfile those variables can be accessed when the image is built. But running containers can’t access values of ARG variables. So, the Variables defines in ARG are also known as build-time variables. 

```sh
ex1:-
FROM ubuntu
ARG NAME=JAVA
ARG MAVEN_HOME=/opt/maven

ex2:-

FROM ubuntu
ARG VERSION
RUN apt update -y && apt install openjdk-$VERSION-jdk -y

$ docker build -t sample_image --build-arg VERSION=8 . (use --build-arg <variable>=<value> flag if you haven't define value for variable in ARG)
```
9. USER INSTRUCTION:
The USER instruction specifies a user that the image should be run as;

```sh
ex:-

FROM ubuntu
RUN useradd anil
USER anil
```

