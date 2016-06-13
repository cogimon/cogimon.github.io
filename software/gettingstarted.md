---
layout: default
title: Getting Started
section: builder/gettingstarted
---

<div class="page-header">
  <h1>Getting Started</h1>
<!--   To get a feel for how the CogiMon modeling toolchain shall work and how you can use it to design control architecture for hybrid force and motion controllers, check out this intro video: -->
</div>

Preliminary notes for replication of the KUKA LWR simulation software and the Ortenzi et al. experiment. Please check back often as we continuously extend this documentation.

#### Software Installation

The CogIMon Simulation Architecture (CoSimA) with support for IIT's COMAN and the KUKA LWRIV+ robot is modeled in a Cognitive Interaction Toolkit (CITk) distribution, which is available [here](https://toolkit.cit-ec.uni-bielefeld.de/systems/versions/cogimon-minimal-simulation-distribution-nightly). It has so far been tested on Ubuntu Trusty (LTS 14.04) using Gazebo 6.5 and OROCOS-RTT 2.8. MacOS and Ubuntu 16.04 will also be supported.

##### Bootstrap the CITk environment

1. Setup the CITk toolchain on your machine according to the instructions [here](https://toolkit.cit-ec.uni-bielefeld.de/tutorials/bootstrapping)

##### Install Gazebo Simulation environment  

###### Add Gazebo repositories for binary installation (recommended)

1. Setup your computer to accept software from packages.osrfoundation.org.

    ***Note:*** there is a list of [available mirrors](https://bitbucket.org/osrf/gazebo/wiki/gazebo_mirrors) for this repository which could improve the download speed.

        sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

1. Setup keys.

        wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

1. Update Package Cache.

        sudo apt-get update

[In case you experience any problems at this stage, please consult the Gazebo installation  guidelines](http://gazebosim.org/tutorials?tut=install_ubuntu&cat=install)

##### Install CogIMon Distribution

2. Install the cogimon-minimal-nightly distribution as explained [here](https://toolkit.cit-ec.uni-bielefeld.de/systems/versions/cogimon-minimal-simulation-distribution-nightly)

Pleas note the following:

* You need to replace the ```$prefix``` in the build generator arguments with the expanded directory location
* You may add ```--cache-directory``` to the build generator command in order to speed up repeated job generation
* You may replace your password with the Jenkins API token that can be retrieved from your Jenkins user profile

A full example of the command line call may look as follows:

```bash
./jenkins/job-configurator --on-error=continue -d ./citk/distributions/cogimon-minimal-nightly.distribution -t './citk/templates/toolkit/*.template' -u ndehio -a 8c4ccaed525d91b0ea9de6f94bdbdd31 -D toolkit.volume=/vol/coman --delete-other --cache-directory=/home/ndehio/.buildgen
```

#### Experiment Reproduction

The experiment can be replicated in your environment if you follow the following steps.

##### Bash Configuration

```bash
# install prefix of the CogIMon distribution
$PREFIX=/vol/cogimon

# tell Gazebo about the CogIMon simulation plugins
export GAZEBO_PLUGIN_PATH=$PREFIX/lib:$PREFIX/lib/orocos/gnulinux/rtt_gazebo_system:$PREFIX/lib/orocos/gnulinux/rtt_gazebo_deployer_world_plugin

# tell OROCOS_RTT also about the plugin location
export RTT_COMPONENT_PATH=$PREFIX/lib/orocos

# if you want to include your own controllers / plugins, you need to 
# add your developer workspace to the RTT path
#export RTT_COMPONENT_PATH=$RTT_COMPONENT_PATH:/homes/dwigand/code/cogimon/osf-controller/build/src/orocos

# these variables also need to be set
export LD_LIBRARY_PATH=$PREFIX/lib
export PKG_CONFIG_PATH=$PREFIX/lib/pkgconfig
```

##### Experiment Execution

The following commands assume that they are executed in a command shell with an environment configured as indicated above.

###### 1. Start the Gazebo Server with the required plugins

```bash
gzserver -s $PREFIX/lib/orocos/gnulinux/rtt_gazebo_system/librtt_gazebo_system.so $PREFIX/etc/cogimon-scenarios/scenario-wipe-board/world/scn-wipe-board-vertical.world
```

###### 2. Start the Gazebo Client in a new terminal

```bash
gzclient &
```

###### 3. Load the KUKA LWR model plugin

```bash
$PREFIX/bin/rsb0.13 call 'socket:/GazeboDeployerWorldPlugin/spawnModel("/vol/toolkit/cogimon-minimal-lwr-nightly/etc/lwr-robot-description/lwr-robot.urdf")'
```

###### 4. Load and configure the experiment plugins and start the simulation

```bash
$PREFIX/bin/rsb0.13 call -l $PREFIX/share/rst0.13/proto/sandbox/rst/cogimon/ModelComponentConfig.proto 'socket:/GazeboDeployerWorldPlugin/deployRTTComponentWithModel(pb:.rst.cogimon.ModelComponentConfig:{component_name:"lwr_gazebo" component_type:"LWRGazeboComponent" component_package:"rtt-gazebo-lwr-integration" model_name:"kuka-lwr" script:"/vol/toolkit/cogimon-minimal-lwr-nightly/etc/cogimon-scenarios/scenario-wipe-board/script/scn-wipe-board-short.ops"})'
```

##### Notes

If you run the experiment for the first time, an configuration file named `rsb.conf` needs to be placed in the `$HOME/.config` directory of the user executing the simulation. An exemplary configuration is shown in the following:

```
[transport.socket]
enabled = 1    # ENABLE to use socket transport!
# SETUP the correct mapping between types!
converter.cpp.".rst.dynamics.Pressures" = rst::dynamics::Pressures
converter.cpp.".rst.geometry.Lengths" = rst::geometry::Lengths
converter.cpp.".rst.geometry.Pose" = rci::Pose
converter.cpp.".rst.signalprocessing.InstantaneousPhase" = rst::signalprocessing::InstantaneousPhase
converter.cpp.".rst.cbse.ComponentState" = cca::ComponentState
converter.cpp.".rst.dynamics.Forces" = rci::Forces
converter.cpp.".rst.dynamics.JointImpedance" = rci::JointImpedance
converter.cpp.".rst.signalprocessing.InstantaneousPhase" = rst::signalprocessing::InstantaneousPhase
converter.cpp.".rst.cbse.ComponentState" = cca::ComponentState
converter.cpp.".rst.dynamics.JointTorques" = rci::JointTorques
converter.cpp.".rst.dynamics.Wrench" = rci::Wrench
converter.cpp.".rst.geometry.Translation" = rci::Translation
converter.cpp.".rst.kinematics.JointAccelerations" = rci::JointAccelerations
converter.cpp.".rst.kinematics.JointAngles" = rci::JointAngles
converter.cpp.".rst.kinematics.JointVelocities" = rci::JointVelocities
converter.cpp.".rst.math.VectorDouble" = rci::Doubles
converter.cpp.".rst.cbse.Tick" = cca::timing::Tick
 
[transport.spread]
enabled = 0    # DISABLE because we are using the socket transport!
host = localhost
port = 4803
 
[plugins.cpp]    # LOAD the required plugins!
load = rsbintrospection:rsbrstconverterssandbox:rsbrstconvertersrci:rsbrstconvertersstable
 
[introspection]
enabled = 1    # enable to make use of introspection tools (optional).
```

<!-- TODO:
* Add link and explanation to CITk distribution / experiment -->

<!-- <p>
  <iframe id="player" type="text/html" width="640" height="390"
      src="http://www.youtube.com/embed/AuTo_6id3J8?enablejsapi=1&origin=http://docs.jetstrap.com/"
        frameborder="0"></iframe>

</p>
 -->
