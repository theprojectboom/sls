# Systems Level Simulator

A systems level simulator that we can test the Avionics team’s state estimator 
performance, navigational performance, and etc; and use SLS to loop into the test flight data 
to better prepare for the flight day where we attempt to break the record. The use case is 
not limited to just Avionics team. As the simulation achieves high nough fiedelity in transonic 
and supersonic conditions, we can use the simulator as part of the design iteration process.

## Contribute
* See Issues page, and Projects page of this repositroy for tasks to do
* Discussions in #avionics_sls, and #simulation_egnine channel

## Dependencies
### First approximation
* Operating System
  * Linux x64
  * Maybe macOS
* Hardware GPU (Integrated GPU is OK, VM does not work)
* 8 GB RAM

### Applications
* docker daemon, to run
  * theprojectboom/px4 (avionics software, focus of development work)
  * theprojectboom/fdm (JSBSim)
* QGroundControl (for control commands during test & flight)
* FlightGear (for visualization)

## Getting Started
### Downloads (hurry up & wait)
```
git clone --recurse-submodules https://github.com/theprojectboom/sls  # 1 GB. px4 has 13+ submodules
docker pull theprojectboom/px4:latest  # 3 GB
docker pull theprojectboom/fdm:latest  # 0.5 GB
```

### Running
#### QGroundControl
Download, install, and run [QGroundControl](http://qgroundcontrol.com/).
#### JSBSim
```
docker run -p ...:... -p : -p :  theprojectboom/px4:latest
```
#### Px4 (& development)
```
docker run --volume ...:/opt/px4 -p ...:... -p : -p :  theprojectboom/px4:latest bash
```
#### FlightGear

Need to configure to run with external fdm (flight dynamics model).
Port 5500, default for 


### About docker
Docker runs containers, a kind of lightweight virtual machine on your computer. The containers allow
easy sharing or deployment of software without installing libraries or compiled binaries onto your
system permanently. On macOS, and possibly other platforms, the containers do run inside a virutal
machine. The easiest and best way to interact with the containers (execute commands, inspect
contents, start and stop) is through the `docker` executable and the subcommands it accepts. More
information can be found with `man docker[-cmd]`, `docker [cmd] --help`, or online.

You can download (or create) docker images with:
* `docker pull [img[:version]]`
* `docker build`

You can start a container, as in instance of an image, with:
* `docker run [options] [img[:version]] [command]`
* `docker start [options] [container]`

You can list running (or --all) containers and images with:
* `docker ps [-a]`
* `docker containers`
* `docker images`

You can run a command in a container with:
* `docker exec [options] [container] [command]`
* `docker logs [options] [container]`

You can stop, remove a container with
* `docker stop [container]`
* `docker kill [container]`
* `docker rm [container]` - removes container
* `docker rmi [image]` - removes image

### More notes for contributors
* http://wiki.flightgear.org/Property_Tree/Sockets
* For Px4, note, despite the docs mentioning 14540 ? or similiar, the port 18540 is in use instead
