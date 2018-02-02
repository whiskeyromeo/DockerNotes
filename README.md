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
  docker build -t <whatever-name-you-want> <dir>
```

## Using Docker Swarm
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

From the docker site:
> run `docker swarm init` to enable swarm mode and make your current machine a swarm manager, then run `docker swarm join` on oher machines to have them join the swarm as workers

### Running swarms on multiple VMs
* Get your VMs up and running
```
  docker-machine create --driver virtualbox myvm1
  docker-machine create --driver virtualbox myvm2
  docker-machine ls
```  
* Make one VM the manager of a new swarm while the other is made a follower
  - There are two options for execution, first from the default docker-machine
    ```
      docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip>
      docker-machine ssh myvm2 "docker swarm join ...<instructions from prior command>..."  
    ```
  - Or by using the specified docker-machine
    ```
      docker-machine env myvm1
      eval $(docke-machine env myvm1)
      docker-machine ls     # for verification
      ...Commands from quotes above, repeating second command to get myvm2 to join
    ```
* Deploy as specified above under '**Using Swarms**', the load balancing should then take place between VMs so that
any replicas will be distributed across the machines. These instances can then be accessed using ssh as specified above. 

### Adding services

* If you check out `docker-compose.yml` you will see three services, web, visualizer, and redis. The elements down the tree from each of these services represent the configuration for each service. To add a service, simply follow the proper configuration steps and add it in a similar fashion. 
* If you have a swarm with the visualizer open running, you can visualize it on port 8080 of your docker-machine ip
  - NOTE : Make sure you are on the leader's ip

* Notice that in `docker-compose.yml` under the redis and visualization services we are running these services on the manager, in the case of the visualizer this restricts our access to the GUI to the port on the manager's ip. In the case of redis, we are accessing an arbitrary directory `/data` located in the containers filesystem, which would be wiped if the container was ever redeployed. This constrains redis to using the same host and lets the container access the ./data directory on the specified host, enabling continuity between container deployments.
* In order to create the ./data directory, we ssh into the manager and deploy the docker-compose.yml with the redis service
   ```
      mkdir ./data
      docker stack deploy -c docker-compose.yml <my-image-name>
      docker service ls     # to verify that the proper number of services were created with the correct number of replicas
   ```
* Visiting our managing or member swarm ips we should be able to access data from our redis store. If we visit 
the visualizer, we should see the number of replicas of our original service( container) running as well as the visualizer and redis services. 


## Deploying containers using AWS

* Assuming you have a docker-compose set up inside your app which you want to deploy and have done some base configurations to your AWS account including setting up IAM management, you can deploy a new EC2 instance using docker-machine. First set up a new VM, using your credentials which should be located at ~/.aws/credentials if you are on mac or linux.
```
  docker-machine create --driver amazonec2 <your-desired-ec2-instance-and-vm-name>
  docker-machine env <that-name-you-chose>
  eval $(docker-machine env <that-name-you-chose>)
```

  

