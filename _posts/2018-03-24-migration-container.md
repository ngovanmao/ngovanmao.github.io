---
layout: post
title:  "Migration Docker Container in Edge Computing"
date:   2018-03-24 12:44:17 +0800
categories: jekyll update
commentIssueId: 1
---
# Motivation of migration in edge computing
In edge computing, mobile users can offload computational tasks to near by server, called edge-server or edge-node, instead of sending the request to centralized cloud. 
In other words, the computation service is distributed to closer end-user (EU) location.
By pushing computation close to EUs, the end-to-end (E2E) delay to response the requests or processing the data is reduced significantly. 
If the computation is located right in the associated access point (AP) or base station (BTS) (in cellular network), the E2E delay is the same as one-hop delay, which is without the transmission time to/from the centralized cloud. 
With the advance of virtualization technology, e.g., virtual machine (VM), or container ([Docker](https://www.docker.com/), LXC, rkt CoreOS), we can deploy multiple services in the edge node without concern about dependency library.
In the edge computing, there are two approaches for deploying services, one uses VM as a basic unit for isolation deployment, the other use container. 
Compare to VM, container is much lighter weight, so we can deploy/destroy quickly the service in the edge, and each edge-server can simultaneously host more services than VM.
In the scope of this post, I use Docker container for deploying edge services. 
Since running services are located in edge-node, if the mobile user moves from one place to another place, but the service is still running at the old edge-node. 
The mobile user has to suffer long delay for transmitting requests/responses between the new AP/BTS and the edge node via Wide Are Network (WAN) connection.

# Migration tools
Therefore migration running service is a very important feature in edge computing. 
We can categorized the service in the Internet into two types: **stateless service** and **stateful service**. 
The stateless services, e.g., download content from web server, request global time service, do not require to migrate the running states from the old edge node to a new edge node.
The solution to stateless services is very simple, we can deploy the running service to the potential next edge node in advance, so whenever the EU moves to the new places, it can request contents from the new edge node. 
However, the stateful services, e.g., a database service, a tracking person using surveillance camera, play game, etc., require consistent running states when the container moves to a new edge node.
In order to keep the running states consistent between the old and new edge node, we migrate the running container from the old to the new edge node. 
Migration container was discussed in 2008, Andrey Mirkin *et al.* proposed container checkpointing and live migration in Linux symposium. 
The tool [CRIU (Checkpoint/Restore In Userspace)](https://criu.org/Main_Page) is an open source software for checkpointing and restoring a running application in Linux OS.
CRIU can checkpoint/save/dump the states of process (tree of processes) into a set of files (*.img) which can be used to restore/restart the process(es) at a later point in time and can be in a different machine.
With this general feature, it can apply to application of live migration which dumps snapshots and migrates the dump files to another host, and then restore the container. 

# How to reduce downtime during migration in edge computing
Now, we will look at an example of migrating a Openface container, which provides face recognition, from one edge node to another. 
The two edge node are connected via WAN connection with bandwidth from 7.2 Mbps to 184.5 Mbps, according to [Akamai reporti Q1 2017](https://www.akamai.com/state-of-the-internet-report/), and latency on average around 50 ms.
Because the connection between two edge nodes is slow compare to the connection between hosts within datacenter, we need to reduce the transmission time for transmitting the checkpointed files. 
There are two approach for live migration: pre-copy, post-copy, and [combining pre-copy and post-copy migration](https://lisas.de/~adrian/?p=1253). 

The pre-copy approach is to pre-dump (`criu pre-dump`) memory and process multiple times and each time only the changed memory pages are written to the checkpoint directory, and trafer to the destination host. 
Note that the container is still running during pre-dump. 
Last pre-dump is assumed the changed memory pages is small enough to reduce transmission time and down time service as well. 

The other approach for reducing downtime is post-copy that the container is stop in the source edge node, and immediately start a container with limitted transferred states, e.g., CPU info, configuration, the required memory pages have not been transferred before restoring process, but on demand. 
The new edge node will halt the container until the requested memory pages are transferred to the new edge node.
CRIU supports [lazy migration](https://lisas.de/~adrian/?p=1287) mode (depends on `userfautlfd` being available on the Linux kernel), for more details you can refer to article [userfaultfd](https://criu.org/Userfaultfd).

If we use Docker container, *docker-cli* has an option to use CRIU for checkpointing and restoring a running container. 
Following, I will show step by step to do a migration between two edge nodes with docker-cli and pre-installed CRIU. In the time I write this post, I use Docker version 17.03.2-ce, CRIU version 3.7.
First, we set up ssh-key to all edge nodes, so that they can transfer file via ssh without requirement of authentication. 
The destination edge node will open a UDP socket server for listenning the instruction from the source edge node, the following instruction will do:
* Pull the docker container image from registry, the container image name is given by the source edge node.

    `sudo docker pull <container-image>`
* Create the container after pulling (not run or start the container).

    `sudo docker create --name <local-name-container> <container-image>`
* Recreate the checkpointed files from delta images and the pre-dump images. 

    `xdelta patch <delta-dump> <pre-dump> <stop-dump>`
* Restore the docker container.

    `sudo docker start --checkpoint=<stop-dump> --checkpoint-dir=<dump_dir> <local_name_container>`
* (Optional) Send a command to the source node to delete the container images and checkpointed directories.

The source edge node will do the following steps:
* Send a command to the destination edge node to instruct it download the base container image from Docker registry (e.g., docker hub).
* Checkpoint and leave the container running, we use an option: `--checkpoint-dir=<pre-dump_dir>`, and option `--leave-running`. See more details in [docker docs](https://docs.docker.com/engine/reference/commandline/checkpoint/).

    `sudo docker checkpoint create --leave-running --checkpoint-dir=<dump_dir> <local-name-container> <pre-dump>`
* Transfer the checkpointed files to the destination edge node.

    `rsync -az <dump_dir>/<pre-dump> <user@destIP:dest_pre-dump_dir>`
* Checkpoint and stop the container.

    `sudo docker checkpoint create --checkpoint-dir=<dump_dir> <local-name-container> <stop-dump>`
* Transfer the configuration files to destination node. At the same time, create delta binary ([xdelta](http://xdelta.org/)) between the pre-dump checkpointed images and stop checkpointed images.

    `xdelta delta <pre-dump> <stop-dump> <delta-dump>`
* Transfer the binary differences to the destination node.

    `rsync -av <dump_dir>/<stop-dump> <user@destIP:dest_stop-dump_dir>`
* Send a command to the destination to instruct reconstruct the stop checkpointed files and restore the running container with the same states as the source node. 



Container is based on two concepts, i.e., control groups (cgroups) and namespace, in Linux. [Cgroups](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/ch01) allow to allocate resources - such as CPU time, system, memory, network bandwidth, or combinations of these resources - among user-defined groups of tasks (processes) running on a system.


Reference:
[cgroup](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/ch01)





