[Home](README.md)
# Install

Uninstall old package
```bash
sudo apt remove docker docker-engine docker.io containerd runc
```
Installer les dépendances requises
```bash
sudo apt update
sudo apt install ca-certificates curl gnupg
```
Ajouter la clé GPG officielle de Docker
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
Ajouter le dépôt officiel de Docker
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Installer Docker Engine
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Vérifier l'installation
```bash
sudo docker run hello-world
```
Ajouter votre utilisateur au groupe docker (pour eviter d'utiliser `sudo` a chaque fois)
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Activer le démarrage automatique de Docker
```bash
sudo systemctl enable docker
```

## Lazydocker 
```bash
VERSION="0.24.4"
curl -sL "https://github.com/jesseduffield/lazydocker/releases/download/v${VERSION}/lazydocker_${VERSION}_Linux_x86_64.tar.gz" | tar xzf -
sudo mv lazydocker /usr/local/bin/
```

# In cli 
Commande magique ! 
```bash
docker stop $(docker ps -aq)
docker system prune -a --volumes
docker rmi -f $(docker image ls -q)
docker volume rm $(docker volume ls -q)
```

## Create
Network
```
docker network create my-network
```
Volume
```
docker volume create my-volume
```
Image
```
docker build path/toFolderWitchConstainTheDockerfile -t name-of-image
```


## Start
```
docker run <image name>
```
    - -p <Y>:<X> option to expose the container's inner port X to be visible as Y from the Docker host
    - --name Assign a specific name to the docker
    - -d or --detache to run in background
    - --rm to remove it automatically when it stop
    - --interactive --tty directly open the interactive mode where we can send input to the started container
    - -e ENV-VAR=value _creer une variable d'environement_
    - --net=<networkname> _attache le docker a un network_
    - --hostname=my-hostname _change le nom du docker dans le reseau (son propre nom par default)_
    - -v <my-volume || $(pwd)/random/path>:/container/path/to/store _attache un volume au docker ou creer un volume anonyme a partir d'un fichier/dossier_ _en rajoutant :ro a la fin on peut etre en lecture seule_

to restart a stoped docker 
```
docker start <container_id | container_name>
```
    - -a or --attach to start in attached mode


## Infos
Show running container 
```
docker ps
```
    - -a For all container
    - -q show ids 

Show info on specific container
```
git inspect <container_id | container_name> 
```
    - -f '{{.State.Status}}' for the state of the container
    - -f '{{.NetworkSettings.IPAddress}}' get the IP address of a container running on the default bridge network 

Show the log of the docker 
```
git logs <container_id | container_name>
```
    - -f print the logs and follows the log output if new logs are created

List all processes running inside a container
```
docker top <container_id || container_name>
```

Get metrics on hardware usage for all containers (memory, cpu, io)
```
docker stats
```

Get metrics on hardware usage for one container
```
docker stats <container_id || container_name>
```

See where volume has physicallystored the data
```
docker volume inspect <volume-name>
```


## Stop
Stop running container 
```
docker stop <container_id | container_name>
```
- That gently stop them if it's don't work replace `stop` by `kill`

Remove a container by name or id
```
docker rm <container_id || container_name>
```
- -f remove a container, even if it is running

Change `<container_id || container_name>` by `$(docker ps -aq)` the remove all containers

Remove all stopped containers
```
docker system prune -a
```
- --volumes pour supprimer les potenciels volumes

Remove an image
```
docker rmi <image_id>
```

Remove all unused images
```
docker image prune
```

Remove all images
```
docker rmi -f $(docker image ls -q)
```

# Docker file 

## `FROM`
Always start with `FROM ***` always set a version with `:vesion` otherwise docker will use `:latest` with could cause problem
- `debian`, `ubuntu` or `alpine` are famous linux distrib
- `mysql` or `nginx` are also popular
- `scratch` pour partir de 0


## `WORKDIR`
Change the working directory to a given one 

## `COPY a b` 
copy the folder/file a in the image at the emplacement b 

## `RUN ***`
Run the given commend in the image 

## `EXPOSE ****`
Expose the deployed app on the port \*\*\*\* of the docker

## `ENTRYPOINT ["command", "-arg"]`
Set the command that will start the docker 

Ex :
```
FROM maven:3.8-amazoncorretto-17
WORKDIR /tmp/build
COPY . .
RUN mvn package -DskipTests

WORKDIR /app
RUN mv /tmp/build/target/*.jar /app/app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

# Docker compose 
Permet de deployer plusieurs docker en même temps
`docker_compose.yml`

At the start put a `version` for exemple `version: "3.7"`

For launch it :
```
docker compose up
```
- -d to run in backround 
- --build to build images 
- --force-recreate to delete old docker before running
- -f to change the default interpretate file (docker-compose.yml)

For down all docker
```
docker compose down
```
- -v to remove named volumes

## `service`
Section where we declare all services

## `image: ***`
we can indicate the image with this param

## `environment`
Section where we declare environment variables 

Ex:
```yaml
environment:
    MYSQL_USER: cdb-user
    MYSQL_PASSWORD: cdb-password
```

## `volumes`
Section in services where we can allow to link volumes 
```yaml
volumes:
    - my-volume:/var/lib/mysql  # named volume
    - ./sql:/docker-entrypoint-initdb.d:ro  # bind-mount volume read only
```

and for create named volumes you can create section behind `services`
```
volumes:
  my-volume:
```

## `networks`
Work the same way that volumes does but for network

## `build`
Section where we put build instruction like the `context` or the `dockerfile`
```
    build:
      context: ./folder
      dockerfile: Dockerfile
```

## `ports`
Section where we put all <Y>:<X> ports correspondance 
```
    ports:
      - "8090:8080"
```

## `depends_on`
Section where we put the services on witch depends the service
```
    depends_on:
      - db
```

## Create

Network
```bash
docker network create my-network
```
Volume
```bash
docker volume create my-volume
```
Image
```bash
docker build path/toFolderWitchConstainTheDockerfile -t name-of-image
```


## Start
```bash
docker run <image name>
```
- -p <Y>:<X> option to expose the container's inner port X to be visible as Y from the Docker host
- --name Assign a specific name to the docker
- -d or --detache to run in background
- --rm to remove it automatically when it stop
- --interactive --tty or -it directly open the interactive mode where we can send input to the started container (oposite to -d)
- -e ENV-VAR=value _creer une variable d'environement_
- --net=<networkname> _attache le docker a un network_
- --hostname=my-hostname _change le nom du docker dans le reseau (son propre nom par default)_
- -v <my-volume || $(pwd)/random/host/path>:/container/path/to/store> _attache un volume au docker ou creer un volume anonyme a partir d'un fichier/dossier_ _en rajoutant :ro a la fin on peut etre en lecture seule_

to restart a stoped docker 
```bash
    docker start <container_id | container_name>
```
- -a or --attach to start in attached mode


## Infos
Show running container 
```bash
docker ps
```
- -a For all container
- -q show ids 

Show info on specific container
```bash
git inspect <container_id | container_name> 
```
- -f '{{.State.Status}}' for the state of the container
- -f '{{.NetworkSettings.IPAddress}}' get the IP address of a container running on the default bridge network 

Show the log of the docker 
```bash
git logs <container_id | container_name>
```
- -f print the logs and follows the log output if new logs are created

List all processes running inside a container
```bash
docker top <container_id || container_name>
```

Get metrics on hardware usage for all containers (memory, cpu, io)
```bash
docker stats
```

Get metrics on hardware usage for one container
```bash
docker stats <container_id || container_name>
```

See where volume has physicallystored the data
```bash
docker volume inspect <volume-name>
```

Execute a command in the container
```bash
docker exec <container_id || container_name> bash
```


## Stop
Stop running container 
```bash
docker stop <container_id | container_name>
```
- That gently stop them if it's don't work replace `stop` by `kill`

Remove a container by name or id
```bash
docker rm <container_id || container_name>
```
- -f remove a container, even if it is running

Change `<container_id || container_name>` by `$(docker ps -aq)` the remove all containers

Remove all stopped containers
```bash
docker system prune -a
```
- --volumes pour supprimer les potenciels volumes

Remove an image
```bash
docker rmi <image_id>
```

Remove all unused images
```bash
docker image prune
```

Remove all images
```bash
docker rmi -f $(docker image ls -q)
```

# Docker file 
It's a pile of layer, if one layer change all layer behind will be rebuild. So less the layer change higher it should be in docker file.

## `FROM`
Always start with `FROM ***` always set a version with `:vesion` otherwise docker will use `:latest` with could cause problem
- `debian`, `ubuntu` or `alpine` are famous linux distrib
- `mysql` or `nginx` are also popular
- `scratch` pour partir de 0
- *Toujours mettre des versions complètes dans les image d.d.d pour avoir quelque chose de fix*

## `WORKDIR`
Change the working directory to a given one 

## `COPY a b` 
copy the folder/file a in the image at the emplacement b 

## `RUN ***`
Run the given commend in the image 

## `EXPOSE ****`
Expose the deployed app on the port \*\*\*\* of the docker

## `ENTRYPOINT ["command", "-arg"]`
Set the command that will start the docker 

Ex :
```
FROM maven:3.8-amazoncorretto-17
WORKDIR /tmp/build
COPY . .
RUN mvn package -DskipTests

WORKDIR /app
RUN mv /tmp/build/target/*.jar /app/app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

# Docker compose 
Permet de deployer plusieurs docker en même temps
`docker_compose.yml`

At the start put a `version` for exemple `version: "3.7"`

For launch it :
```bash
docker compose up
```
- -d to run in backround 
- --build to build images 
- --force-recreate to delete old docker before running
- -f to change the default interpretate file (docker-compose.yml)

For down all docker
```bash
docker compose down
```
- -v to remove named volumes

## `service`
Section where we declare all services

## `image: ***`
we can indicate the image with this param

## `environment`
Section where we declare environment variables 

Ex:
```yaml
environment:
    MYSQL_USER: cdb-user
    MYSQL_PASSWORD: cdb-password
```

## `env_file`
We can pass an entire file directly like that too
```yaml 
env_file:
      - .env
```

## `volumes`
Section in services where we can allow to link volumes 
```yaml
volumes:
    - my-volume:/var/lib/mysql  # named volume
    - ./sql:/docker-entrypoint-initdb.d:ro  # bind-mount volume read only
```

and for create named volumes you can create section behind `services`
```yaml
volumes:
  my-volume:
```

## `networks`
Work the same way that volumes does but for network

## `build`
Section where we put build instruction like the `context` or the `dockerfile`
```yaml
    build:
      context: ./folder
      dockerfile: Dockerfile
```

## `ports`
Section where we put all <Y>:<X> ports correspondance 
```yaml
    ports:
      - "8090:8080"
```
Utiliser une varable pour definir les ports partagé pour eviter les conflis en cas de presence de nombreux projets en local

## `depends_on`
Section where we put the services on witch depends the service
```yaml
    depends_on:
      - db
```

## `restart`
We can put a condition when the image will restart. Ex :
```yaml
    restart: on-failure
```
With `restart: always` or `restart: unless_stopped`, Docker will restart containers even after you reboot your machine.


## Start with special condition 
For exemple we want the back to start after the db so we want to start after the healthcheck db is okay.

## `healthcheck`
Ex test db
```yaml
    healthcheck:
      test: "mysql -u$$MYSQL_USER -p$$MYSQL_PASSWORD -h localhost -e 'show databases'"
      timeout: 20s
      retries: 10
      interval: 2s
```

Depends_on can include condition on service like that
```yaml
    depends_on:
      db:
        condition: service_healthy
```


## Environment 
We can use different docker_compose by env with -f but we can also use `docker-compose.override.yml` witch is read after `docker-compose.yml` and change it
