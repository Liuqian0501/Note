
# part3: Span your service across multiple machines


## Prerequisites

*   [Install Docker version 1.13 or higher](https://docs.docker.com/engine/installation/).

*   Get [Docker Compose](https://docs.docker.com/compose/overview/) as described in [Part 3 prerequisites](https://docs.docker.com/get-started/part3/#prerequisites).

*   Get [Docker Machine](https://docs.docker.com/machine/overview/), which is pre-installed with [Docker for Mac](https://docs.docker.com/docker-for-mac/) and [Docker for Windows](https://docs.docker.com/docker-for-windows/), but on Linux systems you need to [install it directly](https://docs.docker.com/machine/install-machine/#installing-machine-directly). On pre Windows 10 systems _without Hyper-V_, use [Docker Toolbox](https://docs.docker.com/toolbox/overview/).

*   Read the orientation in [Part 1](https://docs.docker.com/get-started/).

*   Learn how to create containers in [Part 2](https://docs.docker.com/get-started/part2/).

*   Make sure you have published the `friendlyhello` image you created by [pushing it to a registry](https://docs.docker.com/get-started/part2/#share-your-image). We’ll be using that shared image here.

*   Be sure your image works as a deployed container. Run this command, slotting in your info for `username`, `repo`, and `tag`: `docker run -p 80:80 username/repo:tag`, then visit `http://localhost/`.

*   Have a copy of your `docker-compose.yml` from [Part 3](https://docs.docker.com/get-started/part3/) handy.


## I.  Introduction

In [part 3](https://docs.docker.com/get-started/part3/), you took an app you wrote in [part 2](https://docs.docker.com/get-started/part2/), and defined how it should run in production by turning it into a service, scaling it up 5x in the process.

Here in part 4, you deploy this application onto a cluster, running it on multiple machines. Multi-container, multi-machine applications are made possible by joining multiple machines into a “Dockerized” cluster called a **swarm**.

## II. Understanding Swarm clusters
A **swarm is a group of machines that are running Docker and joined into a cluster**. After that has happened, you continue to run the Docker commands you’re used to, but now they are executed on a cluster by a **swarm manager**. The machines in a swarm can be physical or virtual. After joining a swarm, they are referred to as **nodes**.

Swarm managers can use several strategies to run containers, such as “emptiest node” – which fills the least utilized machines with containers. Or “global”, which ensures that each machine gets exactly one instance of the specified container. You instruct the swarm manager to use these strategies in the Compose file, just like the one you have already been using.

Swarm managers are the only machines in a swarm that can execute your commands, or authorize other machines to join the swarm as **workers**. Workers are just there to provide capacity and do not have the authority to tell any other machine what it can and cannot do.

Up until now, you have been using Docker in a **single-host mode** on your local machine. But Docker also can be switched into **swarm mode**, and that’s what enables the use of swarms. Enabling swarm mode instantly makes the current machine a swarm manager. From then on, Docker will run the commands you execute on the swarm you’re managing, rather than just on the current machine.

## III. Set up your swarm (Updating)
*   [Local VMs (Mac, Linux, Windows 7 and 8)](https://docs.docker.com/get-started/part4/#local)
*   [Local VMs (Windows 10/Hyper-V)](https://docs.docker.com/get-started/part4/#localwin)

