# Generacion del container en Docker Engine con Docker Compose

## A modo de ejemplo, los pasos en mi PC

Aqui usamos unos paquetes de java y compilamos con gradle, solo para mostrar lo que se puede hacer.

### Genero el archivo docker para instanciar el container:
```
# ====================================================
# Dockerfile para generar contenedor con los paquetes 
# necesarios para la compilacion de java 
#
# Autor: Cepxio
# Image: CentOS 7.2
# Package to install: java-1.8.0-openjdk.x86_64
# ====================================================

# La imagen desde donde vamos a levantar el contenedor.
FROM centos
MAINTAINER cepxio <sostapowicz@cmd.com.ar>
# Con RUN ejecutamos lo que sea en el OS
RUN yum install -y java-1.8.0-openjdk-devel.x86_64 
RUN yum install -y make
# Tambien se pueden exportar variables con ENV, que precisamos persistir en el contenedor.
ENV JAVA_HOME='/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.101-3.b13.el7_2.x86_64'
ENV SPRING_PROFILES_ACTIVE='prod'
```

### Ejecuto el start up de la imagen:

`$ docker-compose up -d`

### Luego, genero el archivo para iniciar el contenedor con compose:

```
---
version: '2'
services:
  reward:
    image: centos-java1.8:rewards
    build: /home/cepxio/tmp/CentOS-Reward-Compilador/
    volumes:
      - /home/cepxio/tmp/mgp-reward-server:/usr/local/src/mgp-reward-server
      - /home/cepxio/tmp/gradles/gradle_rewards/.gradle:/root/.gradle
    container_name: rewards
```

### Con el ejemplo anterior el contenedor levanta y automaticamente quedamos dentro con un pseudo tty
### Con el siguiente ejemplo el contenedor levanta, compila y se detiene, dejando el war|jar en el lugar especificado

```
---
version: '2'
services:
  reward:
    image: centos-java1.8:rewards
    build: /home/cepxio/tmp/CentOS-Reward-Compilador/
    volumes:
      - /home/cepxio/tmp/mgp-reward-server:/usr/local/src/mgp-reward-server
      - /home/cepxio/tmp/gradles/gradle_rewards/.gradle:/root/.gradle
    container_name: rewards
    command: bash -c "cd /usr/local/src/mgp-reward-server && ./gradlew bootRepackage"
```

### Para la version 1 del YML, inicio con compose el contenedor (parado en el directorio donde esta el yml; ejecuto la compilacion con gradle a mano)

```
cepxio@corvette:~/tmp/CentOS-Reward-Compilador$ docker-compose run reward 
[root@fc0846a917a1 /]# cd /usr/local/src/
[root@fc0846a917a1 src]# ll
total 4
drwxr-xr-x 12 1000 1000 4096 Aug  9 14:21 mgp-reward-server
[root@fc0846a917a1 src]# cd mgp-reward-server/
[root@fc0846a917a1 mgp-reward-server]# rm build/libs/reward-0.0.2-SNAPSHOT.war
rm: remove regular file 'build/libs/reward-0.0.2-SNAPSHOT.war'? y
[root@fc0846a917a1 mgp-reward-server]# 
[root@fc0846a917a1 mgp-reward-server]# 
[root@fc0846a917a1 mgp-reward-server]# ./gradlew bootRepackage
:cleanResources
:processResources
:compileJava UP-TO-DATE
:compileScala UP-TO-DATE
:classes
:findMainClass
:war
:bootRepackage

BUILD SUCCESSFUL

Total time: 11.478 secs

This build could be faster, please consider using the Gradle Daemon: https://docs.gradle.org/2.14/userguide/gradle_daemon.html
[root@fc0846a917a1 mgp-reward-server]# 
```

### Listop!