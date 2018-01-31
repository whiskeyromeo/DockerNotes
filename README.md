# DockerNotes
Notes for configuration of Docker containers

## Basic
Go to the site and follow the instructions to set up docker on your machine
if you haven't previously installed.

## QuickRef
If you've got setup done,

- To Restart a machine
> ```docker-machine restart```
- To stop your machine
> ```docker-machine stop```

- To allow interaction with containers select your relevant
docker-machine config
> ```docker-machine env default```
- Configure your shell
> ```eval $(docker-machine env default)```

- To list your images
> ```docker images```

- Container work
```
  docker ps -a    # List all containers
  docker stop <first-3-or-4-digits-from-id>   # stop a container
  docker rm <first-3-or-4-digits-from-id> # delete a container
  docker run -d -p 3000:80 <my-docker-image-name>  # Run a new container with port 80 exposed internally and port 3000 externally


```

