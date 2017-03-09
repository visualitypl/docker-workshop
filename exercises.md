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

### Before: 5432 port must be open! Stop PostgreSQL server.

But let's learn some basics. Go to [https://hub.docker.com/explore/](https://hub.docker.com/explore/), scroll down and click postgres. This is a offical PostgresSQL image hosted on Docker Hub. You can notice that there are a lot of versions under "_Supported tags and respective Dockerfile links_" section.

To run an image with a specific tag, you have to add `:tag_name` after image name, eg: `ubuntu:16:04`.  Now we are going run PostgreSQL 9.5 container.

```
docker run -d postgres:9.5
```

We used `-d` option. It tells to docker to run container as a background process in a “detached” mode. To be sure that this container is working, type: `docker ps.` We've used without `-a`  option, because we want see only currently working containers. You should see something like this:

```
root@docker-workshop:~# docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
f3b98375a644        postgres:9.5        "docker-entrypoint..."   2 minutes ago       Up 2 minutes        5432/tcp            trusting_bose
```

We should check if we can login to our new SQL server. You can use `psql` command or GUI program. Default user and database is `postgres`  and password `mysecretpassword`.

```
root@docker-workshop:~# psql -h localhost -U postgres -d postgres

psql: could not connect to server: Connection refused
        Is the server running on host "localhost" (127.0.0.1) and accepting
        TCP/IP connections on port 5432?
```

The problem is that Docker run containers isolated from host network, so without binding container ports to the host we cannot connect to them. More: [https://docs.docker.com/engine/userguide/networking/default\_network/binding/](https://docs.docker.com/engine/userguide/networking/default_network/binding/). To solve this, stop previous container and run new one with binding port option. More: [https://docs.docker.com/engine/reference/run/\#expose-incoming-ports](https://docs.docker.com/engine/reference/run/#expose-incoming-ports)

```
docker stop f3b98375a644 # stop container using container id
docker rm trusting_bose # remove container using container name generated automatically

docker run -d -p 5432:5432 postgres:9.5
```

Try again connect to postgresql server. Now it should work. Moreover you can change default user, database and password by putting environment variables into container \(option `-e`\). All those option are described in official postgres repo [https://hub.docker.com/\_/postgres/](https://hub.docker.com/_/postgres/)

Your task is:

1. Run three docker containers with PostgreSQL image in versions 9.3, 9.4, 9.5  \(**Tip**: use different ports\)
2. Containers should run as background processes in a “detached” mode
3. Each container should be named like: postgres9.3 etc \(`--name option`\) and has different user names
4. Check if you can login to each database.

## Exercise 3

**Goal: **Build docker image for Rails app and run it.

In this exercise you will build image for Rails app. To do that create a new rails app: `rails new rails-docker -T`. Got to new directory and create `Dockerfile` file.

Example: [https://docs.docker.com/engine/getstarted/step\_four/](https://docs.docker.com/engine/getstarted/step_four/)

Docker reference: [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

What Dockerfile should:

1. Be based on Ruby image \(**Tip**: `FROM` instruction\)
2. Install nodejs depedency \(**Tip**: `RUN` instruction\)
3. Create an app directory and make it as a workdir \(**Tip**: `WORKDIR`\)
4. First copy only Gemfile and Gemfile.lock \(**Tip**: `COPY`\)
5. Run bundle install
6. Copy all app files
7. Run rails server with binding to 0.0.0.0

After that build image and run it \(remember about port binding\).

**Note: **Think why points 4, 5, 6 are in this order.

## Exercise 4

**Goal: **Push your image do hub registry

