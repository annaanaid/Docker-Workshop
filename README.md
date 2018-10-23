# pre reqs

**Install docker [Get docker](https://www.docker.com/get-started)**

# Warum ist docker spannend?
*"Docker is designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. – Opensource.com"*

- Gleiche Umgebung wie auf Prod (Wenn es läuft, läuft es auch auf Prod) -> Stand alone package
- Gleiche Umgebung für ganzes Team
- Weniger tools auf dem System (Tomcat, Jboss, MySql, ...) - only docker needed
- Mehrere Tool-Versionen testbar (Python hell) -> Isoliert host und Container
- Keine 42 Seiten langen Installationsanleitungen
- Management sidecars

# Andere use cases
- Complette CI/CD-Pipeline auf Knopfdruck, siehe: [CI-Github](https://github.com/marcelbirkner/docker-ci-tool-stack) 
- Jenkins build slaves, one image, no host provisioning needed
- ...

# Basics
![Docker Architecture](https://docs.docker.com/engine/images/architecture.svg)

![Docker Filesystem](https://www.mundodocker.com.br/wp-content/uploads/2015/06/docker-filesystems-busyboxrw.png)

![Lagacy to SaaS](https://mycloudblog7.files.wordpress.com/2013/06/screen-shot-2015-06-09-at-2-13-05-pm1.png)

[Docker use cases](https://www.docker.com/why-docker)
  
## Image vs. Container

Image = CD, .iso, Java.class = immutable

Container = Java Object, instantiiertes Image (program running)

## Dockerfile
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

## docker-compose
compose = "Wie sollen verschiedene Container (Services genannt) zusammenarbeiten?":
```
- Netzwerk
- Volumes
- Commandos
- Images
- Healthchecks
- Ressource limits
- ...
```
Man möchte nicht etliche docker run mit vielen parametern scripten..

`docker-compose [up, down, build, pull, push, ...]`

# *Docker use cases*
## Testing mit verschiedenen Versionen (maven/DBs/...)
`docker run -v ${PWD}:/workdir -w /workdir maven:3.5.4-jdk-8 mvn --version` 

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

## interaktives Arbeiten im Container

`docker run -ti -v ${PWD}:/workdir -w /workdir alpine:latest /bin/sh #bash nicht installiert`



## Laufenden Container analysieren
```bash
docker ps 
docker inspect <containerID>
docker logs -f <containerID>
docker exec -ti <containerID> /bin/bash
```

## Container killen
`docker rm <containerID>`

# Remote debugging
- [Cligithub.com/dockerck](https://github.com/docker/labs/blob/master/developer-tools/java-debugging/IntelliJ-README.md)
- [dzone](https://dzone.com/articles/deploy-to-wildfly-and-docker-from-intellij-using-m)
- [jetbrains](https://www.jetbrains.com/help/idea/deploying-a-web-app-into-wildfly-container.html)
- [JRebel](https://github.com/antonarhipov/petclinic-docker-jrebel)

# HandsOn

`git clone git@gitlab.com:cstrobel/docker-workshop.git`
- cd 01webserber
  - docker-compose.yml und nginx/Dockerfile betrachten, Stichwort build-variablen
  - nginx starten docker-compose up
  - http://localhost:8080/ betrachten
  - docker-compose.yml file editieren und volume "einkommentieren"
  - docker-compose up (aktualisiert die Config)
  - nginx/www/index.html editieren, die Änderungen sind sofort ohne reload sichtbar!
  - compose file editieren, volume mounten und index.html live ändern
- cd ../02volumes
  - docker run alpine:latest kein persistenter storage = :(
  - docker volume create my-vol
  - docker volume ls
  - docker run -d --name devtest -v myvol:/storage alpine:latest
    - ls -Al storage
    - touch storage/test
    - exit
  - docker run -d --name devtest -v myvol:/storage alpine:latest
    - ls -Al storage
- Welche Images habe ich auf meinem Rechner?
- Welche Container laufen aktuell?
- Wie kann Docker meinen Entwicklungsworkflow mittels Docker optimieren?



# misc
`docker system prune -f #clean up`
# Links

- [Best Practices](https://github.com/robrich/docker-hands-on-workshop/tree/master/12-Docker-best-practices)
- [Weiteres HandsOn](https://github.com/robrich/docker-hands-on-workshop)
- [Developer Tools Tutorials hub.docker.com](https://github.com/docker/labs/tree/master/developer-tools)
