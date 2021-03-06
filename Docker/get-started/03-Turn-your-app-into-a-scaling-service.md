
# part3: Turn your app into a scaling service


## Prerequisites

*   [Install Docker version 1.13 or higher](https://docs.docker.com/engine/installation/).

*   Get [Docker Compose](https://docs.docker.com/compose/overview/). On [Docker for Mac](https://docs.docker.com/docker-for-mac/) and [Docker for Windows](https://docs.docker.com/docker-for-windows/) it’s pre-installed, so you’re good-to-go. On Linux systems you will need to [install it directly](https://github.com/docker/compose/releases). On pre Windows 10 systems _without Hyper-V_, use [Docker Toolbox](https://docs.docker.com/toolbox/overview/).

*   Read the orientation in [Part 1](https://docs.docker.com/get-started/).

*   Learn how to create containers in [Part 2](https://docs.docker.com/get-started/part2/).

*   Make sure you have published the `friendlyhello` image you created by [pushing it to a registry](https://docs.docker.com/get-started/part2/#share-your-image). We’ll use that shared image here.

*   Be sure your image works as a deployed container. Run this command, slotting in your info for `username`, `repo`, and `tag`: `docker run -p 80:80 username/repo:tag`, then visit `http://localhost/`.


## I.  Introduction

In part 3, we scale our application and enable load-balancing. To do this, we must go one level up in the hierarchy of a distributed application: the **service**.

In a distributed application, different pieces of the app are called “services.” For example, if you imagine a video sharing site, it probably includes a service for storing application data in a database, **a service for video transcoding in the background after a user uploads something, a service for the front-end**, and so on.

Services are really just “containers in production.” **A service only runs one image**, but it codifies the way that image runs—**what ports it should use, how many replicas of the container should run** so the service has the capacity it needs, and so on. **Scaling a service** changes the number of container instances running that piece of software, assigning more computing resources to the service in the process.

Luckily it’s very easy to define, run, and scale services with the **Docker platform** (I use google cloud platform) – just write a `docker-compose.yml` file.

## II. Your first `docker-compose.yml` File
A `docker-compose.yml` file is a YAML file that defines how Docker containers should behave in production.

### `docker-compose.yml`

Save this file as `docker-compose.yml` wherever you want. Be sure you have [pushed the image](https://docs.docker.com/get-started/part2/#share-your-image) you created in [Part 2](https://docs.docker.com/get-started/part2/) to a registry, and update this `.yml` by replacing `username/repo:tag` with your image details.

```yml
    version: "3"
    services:
      web:
        # replace username/repo:tag with your name and image details
        image: username/repository:tag
        deploy:
          replicas: 5
          resources:
            limits:
              cpus: "0.1"
              memory: 50M
          restart_policy:
            condition: on-failure
        ports:
          - "80:80"
        networks:
          - webnet
    networks:
      webnet:
```

This `docker-compose.yml` file tells Docker to do the following:

*   Pull [the image we uploaded in step 2](https://docs.docker.com/get-started/part2/) from the registry.

*   Run five instances of that image as a service called `web`, limiting each one to use, at most, 10% of the CPU (across all cores), and 50MB of RAM.

*   Immediately restart containers if one fails.

*   Map port 80 on the host to `web`’s port 80.

*   Instruct `web`’s containers to share port 80 via a load-balanced network called `webnet`. (Internally, the containers themselves will publish to `web`’s port 80 at an ephemeral port.)

*   Define the `webnet` network with the default settings (which is a load-balanced overlay network).


> 
> You can name the Compose file anything you want to make it logically meaningful to you; `docker-compose.yml` is simply a standard name. We could just as easily have called this file `docker-stack.yml` or something more specific to our project.

## III. Run your new load-balanced app

Before we can use the `docker stack deploy` command we’ll first run:
```
docker swarm init
```
> **Note**: We’ll get into the meaning of that command in [part 4](https://docs.docker.com/get-started/part4/). If you don’t run `docker swarm init` you’ll get an error that “this node is not a swarm manager.”

Now let’s run it. You have to give your app a name. Here, it is set to `getstartedlab`:

```
docker stack deploy -c docker-compose.yml getstartedlab
```

See a list of the five containers you just launched:

<div >

    docker stack ps getstartedlab

</div>

You can run `curl http://localhost` several times in a row, or go to that URL in your browser and hit refresh a few times. Either way, you’ll see the container ID change, demonstrating the load-balancing; with each request, one of the five replicas is chosen, in a round-robin fashion, to respond.
> **Note**: At this stage, it may take up to 30 seconds for the containers to respond to HTTP requests. This is not indicative of Docker or swarm performance, but rather an unmet Redis dependency that we will address later in the tutorial.

## IV. Scale the app
You can scale the app by changing the `replicas` value in `docker-compose.yml`, saving the change, and re-running the `docker stack deploy` command:

```
docker stack deploy -c docker-compose.yml getstartedlab
```
Docker will do an in-place update, no need to tear the stack down first or kill any containers.

Now, re-run the `docker stack ps` command to see the deployed instances reconfigured. For example, if you scaled up the replicas, there will be more running containers.

## V. Take down the app and the swarm
Take the app down with `docker stack rm`:
```
docker stack rm getstartedlab
```

This removes the app, but our one-node swarm is still up and running (as shown by `docker node ls`). Take down the swarm with `docker swarm leave --force`.
> **Note**: Compose files like this are used to define applications with Docker, and can be uploaded to cloud providers using [Docker Cloud](https://docs.docker.com/docker-cloud/) (I use gcloud docker), or on any hardware or cloud provider you choose with [Docker Enterprise Edition](https://www.docker.com/enterprise-edition).

## VI. Recap and cheat sheet (optional)
To recap, while typing `docker run` is simple enough, the true implementation of a container in production is running it as a service. Services codify a container’s behavior in a Compose file, and this file can be used to scale, limit, and redeploy our app. Changes to the service can be applied in place, as it runs, using the same command that launched the service: `docker stack deploy`.

Some commands to explore at this stage:

```sh
docker stack ls              # List all running applications on this Docker host
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker stack services <appname>       # List the services associated with an app
docker stack ps <appname>   # List the running containers associated with an app
docker stack rm <appname>                             # Tear down an application
```
