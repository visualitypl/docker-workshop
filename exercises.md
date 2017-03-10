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

But first let's learn some basics. Go to [https://hub.docker.com/explore/](https://hub.docker.com/explore/), scroll down and click postgres. This is a offical PostgresSQL image hosted on Docker Hub. You can notice that there are a lot of versions under "_Supported tags and respective Dockerfile links_" section.

To run an image with a specific tag, you have to add `:tag_name` after image name, eg: `ubuntu:16:04`.  In our case we are going to run PostgreSQL 9.5 container.

```
docker run -d postgres:9.5
```

We used `-d` option. It tells docker to run container as a background process in a “detached” mode. To be sure that this container is working, type: `docker ps`. We've used without `-a`  option, because we want see only currently working containers. You should see something like this:

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

The problem is that Docker run containers isolated from host network, so without binding container ports to the host we cannot connect to them.  To solve this, stop and remove previous container and run new one with binding port.

> More about network binding [https://docs.docker.com/engine/userguide/networking/default\_network/binding/](#)
>
> More about exposing ports [https://docs.docker.com/engine/reference/run/\#expose-incoming-ports](https://docs.docker.com/engine/reference/run/#expose-incoming-ports)

```
docker stop f3b98375a644 # stop container using container id
docker rm trusting_bose # remove container (you can also manage containers by their names)

docker run -d -p 5432:5432 postgres:9.5
```

Try again connect to postgresql server. Now it should work. Moreover you can change default user, database and password by putting environment variables into container \(option `-e`\). All those options are described in official postgres repo [https://hub.docker.com/\_/postgres/](https://hub.docker.com/_/postgres/)

#### Your task is:

1. Run three docker containers with PostgreSQL image in versions 9.3, 9.4, 9.5  \(**Tip**: use different ports\)
2. Containers should run as background processes in a “detached” mode.
3. Each container should be named like: postgres9.3 etc \(`--name` option\) and has different user names.
4. Check if you can login to each database.

## Exercise 3

**Goal: **Build docker image for Rails app and run it.

In this exercise you will build image for Rails app. To do that, create a new rails app: `rails new rails-docker -T`. Got to new directory and create `Dockerfile` file.

> Dockerfile reference: [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
>
> Example: [https://docs.docker.com/engine/getstarted/step\_four/](https://docs.docker.com/engine/getstarted/step_four/)

#### Your task

* Build Docker image

What Dockerfile should:

1. Be based on Ruby image \(**Tip**: `FROM` instruction\)
2. Install nodejs depedency \(**Tip**: `RUN` instruction\)
3. Create an app directory and make it as a workdir \(**Tip**: `WORKDIR` instruction\)
4. First copy only Gemfile and Gemfile.lock \(**Tip**: `COPY` instruction\)
5. Run bundle install
6. Copy all app files
7. Run rails server with binding to 0.0.0.0 \(**Tip**: `CMD` instruction\)

After that, build image \(you can check if the images exists by command `docker images`\) and run it \(remember about port binding\). Go to the web browser and you should see text: "_Yay! You’re on Rails!"._

**Note: **Think why points 4, 5, 6 are in this order.

#### **Extra task**

Try run this app in production environment. You can define environment variables in Dockerfile.

**Remember**: options defined in `docker run` command will overwrite options in Dockerfile.

## Exercise 4

**Goal: **Push your image do Docker Hub

1. Register in Docker Hub
2. Create a repository 
3. Push image

> Tutorial how to manage Docker Hub [https://docs.docker.com/engine/getstarted/step\_five/](https://docs.docker.com/engine/getstarted/step_five/)
>
> More about `docker push` command [https://docs.docker.com/engine/reference/commandline/push/](https://docs.docker.com/engine/reference/commandline/push/)

#### **Extra task**

Try also create some tags and push them to Docker Hub.

## Exercise 5

**Goal: **Run Rails app in production with connected database container

In this exercise we will run Rails app with database in production environment \(run two containers\). Please download prepared blog Rails app from [https://github.com/visualitypl/docker-workshop-blog-app](https://github.com/visualitypl/docker-workshop-blog-app). This app connects to the database by `DATABASE_URL` env name \(see `config/database.yml`\).

> More about PostgresSQL URIs [https://www.postgresql.org/docs/current/static/libpq-connect.html\#AEN45527](https://www.postgresql.org/docs/current/static/libpq-connect.html#AEN45527)

In this scenario we don't want expose db container port to outside, but connect db to rails app inside docker network. Thanks to docker linking mechanism, containers can easily discover each other by using alias names.

> More about linking containers [https://docs.docker.com/engine/userguide/networking/default\_network/dockerlinks/\#connect-with-the-linking-system](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/#connect-with-the-linking-system)

#### Your tasks:

* Build image from existing code source
* Run Postgresql server 
* Run blog app \(remember about necessary environment variables, binding ports and of course linking\)
* Go to [http://localhost:3000](http://localhost:3000) and you should be able to add new posts

#### **Extra task**

Try the same but without linking mechanism \(use own created network\).

> More about network:
>
> [https://docs.docker.com/engine/userguide/networking/](https://docs.docker.com/engine/userguide/networking/)
>
> [https://docs.docker.com/engine/userguide/networking/work-with-networks/](https://docs.docker.com/engine/userguide/networking/work-with-networks/)

## Exercise 6

**Goal: **Make Exercise 5 easier using Docker Compose

Imagine if you have 6 containers with a lot of options \(env names, links etc\) and want run. Whenever you want run those containers you have to run commands one by one with multiple options. It's hard. Docker Compse allows you to define all containers with configuration in one file and run all them by on command.

> More about Docker Compse https://docs.docker.com/compose/overview/

> Example https://docs.docker.com/compose/rails

> Compose file reference https://docs.docker.com/compose/compose-file/



