# pre reqs

**Install docker [Get docker](https://www.docker.com/get-started)**

# Warum ist docker als Entwickler spannend?
*"Docker is designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. – Opensource.com"*

- Gleiche Umgebung wie auf Prod (Wenn es läuft, läuft es auch auf Prod) -> Stand alone package
- Gleiche Umgebung für ganzes Team
- Weniger tools auf dem System (Tomcat, Jboss, MySql, ...) - only docker needed
- Mehrere Tool-Versionen testbar (Python hell) -> Isoliert host und Container
- Keine 42 Seiten langen Installationsanleitungen

# Andere use cases
- Complette CI/CD-Pipeline auf Knopfdruck, siehe: [CI-Github](https://github.com/marcelbirkner/docker-ci-tool-stack) 
- Jenkins build slaves, one image, no host provisioning needed
- ...

# Basics
![Docker Architecture](https://docs.docker.com/engine/images/architecture.svg)

![Docker Filesystem](https://www.mundodocker.com.br/wp-content/uploads/2015/06/docker-filesystems-busyboxrw.png)

## Image vs. Container

Image = CD, .iso = immutable

Comntainer instantiated image (program running)

## docker-compose
"Wie sollen verschiedene Container zusammenarbeiten (Netzwerk, Volumes, Commandos, Images, ...". Man möchte nicht etliche docker run mit vielen parametern scripten..

`docker-compose [up, down, build, pull, push, ...]`

# Test with different versions (maven)
`docker run -v $(pwd):/workdir -w /workdir maven:3.5.4-jdk-8 mvn --version` 

```bash 
Apache Maven 3.5.4 (1edded0938998edf8bf061f1ceb3cfdeccf443fe; 2018-06-17T18:33:14Z)
Maven home: /usr/share/maven
Java version: 1.8.0_181, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-8-openjdk-amd64/jre
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "4.9.125-linuxkit", arch: "amd64", family: "unix"
```

oder

`docker run -v ${PWD}:/workdir -w /workdir maven:3.5.2-ibmjava-9-alpine mvn --version`

`-v ${PWD}:/workdir -w /workdir` mounted den aktuellen Ordner und setzt das working directory. (Man kann also mvn clean test z. B. direkt ausführen, falls eine pom.xml im Ordner ist)


```bash
Apache Maven 3.5.2 (138edd61fd100ec658bfa2d307c43b76940a5d7d; 2017-10-18T07:58:13Z)
Maven home: /usr/share/maven
Java version: 9-internal, vendor: IBM Corporation
Java home: /opt/ibm/java
Default locale: en_US, platform encoding: ANSI_X3.4-1968
OS name: "linux", version: "4.9.125-linuxkit", arch: "amd64", family: "unix"
```

# interaktives Arbeiten im Container

`docker run -ti -v ${PWD}:/workdir -w /workdir alpine:latest /bin/sh #bash nicht installiert`



# Laufenden Container analysieren
```bash
docker ps 
docker inspect <containerID>
docker logs -f <containerID>
docker exec -ti <containerID> /bin/bash
```

# Container killen
`docker rm <containerID>`

# Dockerfile
siehe nginx/Dockerfile

```bash
FROM # base Image
RUN # exec commands
CMD # default command to start the container
COPY # copy files into image (ADD extracting/downloading)
EXPOSE # port exposure
ENV # environment variable
ARG # build argument
USER # user used
```
## Remote debugging
[Click](https://github.com/docker/labs/blob/master/developer-tools/java-debugging/IntelliJ-README.md)


# misc
`docker system prune #clean up`
# Links
Developer Tools Tutorials
hub.docker.com
https://github.com/docker/labs/tree/master/developer-tools 


# HandOn

`git clone git@gitlab.com:cstrobel/docker-workshop.git`

- nginx starten
- compose file editieren, volume mounten und index.html live ändern
- Welche Images habe ich auf meinem Rechner?
- Welche Container laufen aktuell?
- Wie kann Docker meinen Entwicklungsworkflow mittels Docker optimieren?

`http://localhost:8080`