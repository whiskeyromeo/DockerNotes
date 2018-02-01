# DockerNotes
Notes for configuration of Docker containers

## Basic
Go to the site and follow the instructions to set up docker on your machine
if you haven't previously installed.

## QuickRef

**Default Docker IP is 192.168.99.100 on OSX and Windows, localhost on linux**

If you've got setup done, you will probably have to do the following steps  a lot

### Basic/Frequent Operations
- To Restart a machine
> ```docker-machine restart```
- To stop your machine
> ```docker-machine stop```

- To allow interaction with containers select your relevant
docker-machine config
> ```docker-machine env default```
- Configure your shell
> ```eval $(docker-machine env default)```
- Repo work
```
  docker login                                      # login to dockerhub
  docker tag <image-name> <username>/<repo>:<tag>   # set up an image for upload
  docker push <username>/<repo>:<tag>               # push it up to the repo
```
- Container work
```
  docker ps -a    # List all containers
  docker stop <first-3-or-4-digits-from-id>       # stop a container this way
  docker container stop <digits-from-id>          # or this way
  docker rm <first-3-or-4-digits-from-id>         # delete a container
  docker run -d -p 3000:80 <my-docker-image-name>  # Run a new container in the background with port 80 exposed internally and port 3000 externally
```
- Image work
``` 
  docker images     # list your images
  docker image      # list image commands
  docker image rm <image-name>    # remove an image
```
- Build an image from a Dockerfile -> run in the same dir as the Dockerfile
```
  docker built -t <whatever-name-you-want>
```

## Using Swarms
This requires setting up a docker-compose.yml
```
  docker swarm init   # initialize the swarm
  docker stack deploy -c docker-compose.yml <whatever-name-you-choose>    # start up the swarm with however many replicas were specified
  docker service ls   # list the instances
  docker service ps <whatever-name-you-chose>_web # list the tasks for the container filtered by service ( here using webnet)
  docker container ls -q    # list the tasks for the container
  # the swarm can be accessed at the default( or specified) ip, (get from docker-machine ip)
  # updates to the docker-compose.yml can be deployed via
  docker stack deploy -c docker-compose.yml <whatever-name-you-chose>
  
  # stopping the swarm
  docker stack rm <whatever-name-you-chose>
  docker swarm leave --force
  
```


