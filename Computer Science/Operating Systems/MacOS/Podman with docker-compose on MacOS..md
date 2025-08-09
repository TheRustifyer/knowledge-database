  
> [!info] 
> Podman an alternative to Docker Desktop on MacOS  

Getting **podman** installed and started is super easy.  
## Use `brew` to install it.

```bash
brew install podman
```

# Initialize it

Now since **podman** uses a VM just like the Docker Client on MacOS we need to initialize that and start it.

```bash
podman machine init
podman machine start
```
* Now we are set to go. 

> [!rootless mode]
> This machine is currently configured in rootless mode. If your containers
require root permissions (e.g. ports < 1024), or if you run into compatibility
issues with non-podman clients, you can switch using the following command:

```bash
podman machine set --rootful
```

# (Optional) you can create a symlink so podman can be executed with "docker" command. 

```bash
ln -s /usr/local/bin/podman /usr/local/bin/docker
```

Now most of the commands in **podman** are the same so try `podman images` and you will get a list of images. 

Otherwise the `podman --help` command list all the help you need.  

# `docker-compose` without the docker client for mac.

You can install it using the `brew` command.

```bash
brew install docker-compose
```

When that is done you now should have the ability to use `docker-compose` with `podman`.  

On MacOS the podman project does not expose the `podman.socket` which is similar to `docker.socket`, by default. So to get `docker-compose` working one needs to expose the socket.  

To get the socket running run the following commands.  

1. First we need to find the port it is exposed on in the VM.

```bash
podman system connection ls
``` 

2. Then we need to take that port and create a forward ssh connection to that. 

```bash
ssh -fnNT -L/tmp/podman.sock:/run/user/1000/podman/podman.sock -i ~/.ssh/podman-machine-default ssh://core@localhost:<port to socket> -o StreamLocalBindUnlink=yes

export DOCKER_HOST='unix:///tmp/podman.sock'
```

We expose the `DOCKER_HOST` env variable that is used by `docker-compose`. 

Be aware that if the connection is disconnected one needs to delete/overwrite the `/tmp/podman.socket` to run the forward command. 

> [!INFO]
> 
> ```bash
> ssh -fnNT \
  -L/tmp/podman.sock:/run/user/1000/podman/podman.sock \
  -o StreamLocalBindUnlink=yes \
  -i ~/.local/share/containers/podman/machine/machine \
  core@localhost -p 51604
> ```

# Notes

Overall findings is that if one only runs single images then it is fairly easy to get going using **podman**. But if you rely on the `compose` part to orchestrate the containers in a bigger setup of different images with networking etc. then `podman` is a lot less easy to get working "out of the box". There is a lot of googling involved and then it still seems that there are a lot of the features that are not too easy to get working. I did have a lot of issues getting the right permissions to mount drives into the images. One of the main features with podman is that it is rootless. Which is great but it means that you need to understand what permissions a container needs before it fully works.

I have tried to use the `podman-compose` as the goto instead of `docker-compose`, but I had a hard time even getting it installed, and there were alot of issues where it could not load images from the local repository, so in the end that is why I decided to use `docker-compose` and not `podman-compose`. Another thing is that `podman-compose` is also developed by people not really part of the `podman` community it seems, or it is not set to be the frist choice by the `podman` community. So it seems that it is a project that has its own agenda, and is run by a few people and not as many as the `podman` community.

For now I got it working but I will say that there are many wheels that need tuning and kept updated to have the setup running in a daily development environment. 
So if you, like me, just want to use the tools and not need to finetune all the time, it seems a little like there is a way to go before `podman` takes over the MacOS setup. Next for me is to try to setup everything on my linux laptop and see if this works easier out of the box.