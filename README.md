# Systems Level Simulator

A systems level simulator that we can test the Avionics team’s state estimator 
performance, navigational performance, and etc; and use SLS to loop into the test flight data 
to better prepare for the flight day where we attempt to break the record. The use case is 
not limited to just Avionics team. As the simulation achieves high nough fiedelity in transonic 
and supersonic conditions, we can use the simulator as part of the design iteration process.

## Components
We will use Software-in-the-Loop (sitl) simulations extensively to develop, test, and validate our
[Px4-based flight controller](https://px4.io/).

Simulation flight physics will be handled using [JSBSim](https://github.com/JSBSim-Team/jsbsim) as
our Flight Dynmics Model (FDM). Initially, we will use the base version of JSBSim, but we may
customize the physics for better modeling of critical regimes of flight.

Both in testing and in flight, we will use [QGroundControl](http://qgroundcontrol.com/) for
reporting telemetry and changing flight modes.

During testing, we will use [FlightGear](https://www.flightgear.org) to visualize ground-truth
flight paths.

We will need to adapt an existing script from [Px4's Hardware in the Loop (HIL)
repository](gh/px4/hil) to compare simulator ground truth flight paths with Px4 sensor- and flight
model-derived flight paths as part of testing and validation. It will need to capture telemetry from
Px4 and JSBSim, and proxy data and report differents to QGroundControl, or an automated test bench.

## Contribute
* See Issues page, and Projects page of this repositroy for tasks to do
* Discussions in #avionics_sls, and #simulation_egnine channel

## Dependencies
### First approximation
* Docker on you OS:
  * Linux x64
  * macOS
  * Windows with WSL, git-bash, or cygwin
* Hardware GPU (Integrated GPU is OK, VM does not work)
* 8 GB RAM

### Applications
* Docker daemon, to run
  * theprojectboom/px4
* QGroundControl
* FlightGear (for visualization)

## Getting Started
### Downloads (hurry up & wait)
```
git clone --recurse-submodules https://github.com/theprojectboom/sls  # 1 GB. half .git & half px4, with 13+ submodules
docker pull theprojectboom/px4:latest  # 3 GB
```

### Running
#### Px4
This component, the Flight Controller, is the main focus of our development work.
```
docker run --name boom-px4 --volume `pwd`/.:/opt/boom-sls -p 14570:18570/udp -p 14580:14580/udp -p 4560:4560/udp -p 4560:4560 theprojectboom/px4:latest
```
* NOTE: `docker run -v hostPath:containerPath [...]` - the hostPath must be an absolute path, i.e.
  starting with `/`.

##### JSBSim
I've included a JSBSim executable in the container at `/opt/JSBSim`
If we need to customize the FDM, mount-build-run instructions will be updated here.

#### QGroundControl
Download, install, and run [QGroundControl](http://qgroundcontrol.com/).

#### FlightGear
Need to configure to run with external FDM (Flight Dynamics Model).
Port 5500, default for communication.

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

You can start/restart a container, which is an instance of an image, with:
* `docker run -w /opt/boom-sls [options] [image[:version]] [command]`
* `docker start [options] [existing-container]`

You can list running (or --all) containers and images with:
* `docker ps [-a]`
* `docker containers`
* `docker images`

You can run a command in a container with:
* `docker exec -w /opt/boom-sls [options] [container] [command]`
* `docker logs [options] [container]`

You can stop, remove a container with
* `docker stop [container]`
* `docker kill [container]`
* `docker rm [container]` - removes container
* `docker rmi [image]` - removes image

### More notes for contributors
* http://wiki.flightgear.org/Property_Tree/Sockets
* For Px4, note, despite the docs mentioning 14540 ? or similiar, the port 18540 is in use instead
* We still need to get px4 to used the external JSBSim FDM, using some scripts from https://github.com/px4/hil 
* When doing `git status` note this stack overflow [article](https://stackoverflow.com/questions/4873980/git-diff-says-subproject-is-dirty) and use `git status --ignore-submodules=dirty` or use git version > 2.31 (Q1 2021).
