# Dockerfile examples

1. RUN INSTRUCTION:
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
```sh

2. CMD INSTRUCTION:
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
```sh
FROM ubuntu:latest
RUN apt update -y && apt install wget -y &&  apt install openjdk-8-jdk -y
RUN wget http://apachemirror.wuchna.com/tomcat/tomcat-8/v8.5.54/bin/apache-tomcat-8.5.54.tar.gz
RUN tar -xvf apache-tomcat-8.5.54.tar.gz
CMD ["bash", "/apache-tomcat-8.5.54/bin/catalina.sh", "run"]
ENTRYPOINT echo "hello world"
```
4. COPY INSTRUCTION:
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
```sh
FROM ubuntu
WORKDIR /home/ubuntu
```
7. ENV INSTRUCTION:
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
```sh
ex:-

FROM ubuntu
RUN useradd anil
USER anil
```

