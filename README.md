# DockerNotes
Notes for configuration of Docker containers

## Basic
Go to the site and follow the instructions to set up docker on your machine
if you haven't previously installed.

## QuickRef
If you've got setup done,

To Restart a machine
> ```sh docker-machine restart``
To stop your machine
> ```sh docker-machine stop```

To allow interaction with containers select your relevant
docker-machine config
> ```sh docker-machine env default```
Configure your shell
> ```sh  eval $(docker-machine env default)```
