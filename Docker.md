# Docker notes

- docker run --name u1 -it ubuntu:17.10 /bin/bash   //Correr contenedor e ingresar al shell

# Docker common comands 

- docker ps -a 
- docker run image:tag echo "hello world"
- docker run -it image:tag  \\ Entrar al container
- docker run --rm image:tag \\ Eliminar container
- docker run --name image:tag  \\Nombrar container
- docker run -d            \\ Devolver el ID 
- docker inspect ID 
- docker run -it -p 8888:8080 tomcat:8.0  \\ Mapear puerto 
- docker lohs ID 
- docker history image:tag   \\ Docker layers
- docker commit $Container_id name:tag   \\ Create image locally
- docker build -t name .   \\ Build from Dockerfile
- docker tag container image:tag    \\ Tagear la imagen 
- docker login --username
- docker-machine ls  \\ Docker machine on MAC 
- docker exec -it CONTAINer $Command   \\ ejecutar un comando dentro de contenedor

# Docker file 

- Each RUN instruction generates new layer
- Use && instead 
- CMD : Command that executes when the image starts 
- --no-cache=true   \\ Evitar que se use la cache 
- COPY: Copiar archivos locales al contenedor 
- ADD: Copiar archivos desde internet \ unzip 

# Link Containers (recipient - source) 
- docker run -d --name redis redis:3.2.0   \\ Redis container 
- docker build -t dockerapp:v0.3 .   \\ python app 
- docker run -d -p 5000:5000 --link redis dockerapp:v0.3 
- El link se hace mediante la IP de los containers en el 

# Docker Compose (Way of orchestrate containers)
- Yaml files 
- docker-compose version
- docker-compose up -d
- docker-compose ps
- docker-compose logs
- docker-compose stop
- docker-compose rm
- docker-compose build 
- docker-compuse run $service $command 

# Docker Network ()
- docker network ls
1. Closed - (Isolated network)
- maximim network protection
- #docker run -d --net none busybox sleep 1000
2. Bridge (Default)
- IP range 172.17.0.0/16
- Private network interfaces
- #docker network create --driver bridge my-bridge-net

- #docker network connect $NET $CONTAINER   \\ Conectar contenedores
- #docker network disconnect $NET $CONTAINER 
3. Host (lest protected net model) - all net interfaces defined on the host machine
- #docker run -d --net host busybox sleep 1000
4. Overlay (key-value store like consul)

# Docker Unit Tests
- Run unit test using unitest python
- #docker-compose run dockerapp python test.py

# CI - with Circle CI 
- github project 

# Docker Prod - Docker machine
- docker-machine ls
- Run a container in the cloud : 
- docker-machine create --driver digitalocean --digitalocean-access-token <xxxxx> docker-app-machine
- Example with AWS 
1. Configure aws cli 
- #docker-machine create --driver amazonec2 aws-sandbox
- #docker-machine env aws-sandbox \\ details
- #eval $(docker-machine env aws-sandbox) \\ cofnigure shell 
- docker-machine stop %% docker machine rm 

# Docker Swarm - Manage clusters and containers (Like Kubernetes)
- Create to VMS with docker-machine 
1. node 
2. manager

- #docker swarm init --advertise-addr PUBLICIP  \\ Inicializar manager
- #docker join --token .... \\ Agregar el nodo al manager
  #docker swarm leave   \\ disconect from manager

- Docker stack (group of interrelated services that share dependencies)
1. From docker compose file  # docker stack deploy 
2. In swarm mode
- #docker stack deploy --compose-file prod.yml dockerapp_stack
- #docker stack ls 
- #docker stack services dockerapp_stack 
- #docker stack rm dockerapp_stack
