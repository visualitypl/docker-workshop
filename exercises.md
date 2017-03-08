# Exercises

## Exercise 1

**Goal: **run Docker container and play with bash

First we are going to run **hello-world **container which was prepared by Docker Team. Open your terminal and type below command. See what will happen. 

```bash
docker run hello-world
```

> More information about above comand: [https://docs.docker.com/engine/getstarted/step\_two/](https://docs.docker.com/engine/getstarted/step_two/)



Now let's play with Ubuntu docker image. For example to list root files in Ubuntu container, run **ls **command:

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

As you see, after image name you can put any linux command, which will be invoked inside created container.



```
docker run -it ubuntu bash
```

> Learn more about **-it** on [https://docs.docker.com/engine/reference/run/\#foreground](https://docs.docker.com/engine/reference/run/#foreground)



Let's play with it. Try some unix commands: **ls**, **ps** or create file **touch test.txt**.



To see all containers in you system type:

```
docker ps -a
```

## Exercise 2

**Goal: **Learn more about run command and launch real web app.







