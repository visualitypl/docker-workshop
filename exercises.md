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

## Exercise 2

**Goal: **Learn more about run command and run PostgreSQL servers.

> #### Problem
>
> You are working on multiple projects which use different PostgreSQL versions.



Let's go to https://hub.docker.com/explore/, scroll down and click postgres. This is a offical PostgresSQL image for Docker. You can notice that there are a lot of versions.







