# Exercises

## Exercise 1

**Goal: **run Docker container and play with bash

First we are going to run **hello-world **container prepared by Docker Team. Open your terminal and type below command. See what will happen.

```bash
docker run hello-world
```

> More information about above comand: [https://docs.docker.com/engine/getstarted/step\_two/](https://docs.docker.com/engine/getstarted/step_two/)

Now let's play with Ubuntu docker image. For example to list root files in Ubuntu container, run `ls /`** **command:

```
docker run ubuntu ls /
```

The output should be:

```
bin
boot
...
var
```

As you see, after image name you can put any linux command with arguments, which will be invoked inside created container. If you want run bash console inside it just type:

```
docker run -it ubuntu bash
```

> Learn more about **-it** on [https://docs.docker.com/engine/reference/run/\#foreground](https://docs.docker.com/engine/reference/run/#foreground)

Now you are logged in ubuntu container. Try some unix commands: `pwd`, `ps` or create file `touch test.txt`.

To see all containers in your system type:

```
docker ps -a
```

> More about `docker ps` here: [https://docs.docker.com/engine/reference/commandline/ps/](https://docs.docker.com/engine/reference/commandline/ps/)

## Exercise 2

**Goal: **Learn more about managing containers.

#### Problem

You are working on multiple projects which use different PostgreSQL versions 9.3, 9.4 and 9.5.

#### Solution

Use Docker :\)

Before: 5432 port has to be open!

But let's learn some basics. Go to [https://hub.docker.com/explore/](https://hub.docker.com/explore/), scroll down and click postgres. This is a offical PostgresSQL image hosted on Docker Hub. You can notice that there are a lot of versions under "_Supported tags and respective Dockerfile links_" section.

To run an image with a specific tag, you have to add `:tag_name` after image name, eg: `ubuntu:16:04`.  Now we are going run PostgreSQL 9.5 container.

```
docker run -d postgres:9.5
```

We used `-d` option. It tells to docker to run container as a background process. To be sure that this container is working, type: `docker ps.` We've used without `-a`  option, becase we want see only currently working containers. You should see something like this:

```
root@docker-workshop:~# docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
f3b98375a644        postgres:9.5        "docker-entrypoint..."   2 minutes ago       Up 2 minutes        5432/tcp            trusting_bose
```

We should check if we can login to our new database. You can use `psql` command or GUI program.



You task is:

1. Run three docker containers with PostgreSQL image in versions 9.3, 9.4, 9.5
2. Containers should run as background processes
3. Each container should be named like: postgres9.3 etc



## Exercise 3

**Goal: **Build docker image from existing source code.

#### 



