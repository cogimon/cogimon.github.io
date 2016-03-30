---
layout: default
title: Getting Started
section: builder/gettingstarted
---

<div class="page-header">
  <h1>Getting Started</h1>
<!--   To get a feel for how the CogiMon modeling toolchain shall work and how you can use it to design control architecture for hybrid force and motion controllers, check out this intro video: -->
</div>

Oops. Nothing here, yet. We are constantly adding content, so please check back often.

Notes:

#### CogIMon CITk distribution

is available [here](https://opensource.cit-ec.de/projects/citk)

#### Experiment Reproduction

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

```bash
$PREFIX/bin/rsb0.13 call 'socket:/GazeboDeployerWorldPlugin/spawnModel("/vol/toolkit/cogimon-minimal-lwr-nightly/etc/lwr-robot-description/lwr-robot.urdf")'
```

###### 3. Load the KUKA LWR model plugin

```bash
$PREFIX/bin/rsb0.13 call 'socket:/GazeboDeployerWorldPlugin/spawnModel("/vol/toolkit/cogimon-minimal-lwr-nightly/etc/lwr-robot-description/lwr-robot.urdf")'
```

###### 4. Load the experiment plugins and start the simulation

```bash
$PREFIX/bin/rsb0.13 call -l $PREFIX/share/rst0.13/proto/sandbox/rst/cogimon/ModelComponentConfig.proto 'socket:/GazeboDeployerWorldPlugin/deployRTTComponentWithModel(pb:.rst.cogimon.ModelComponentConfig:{component_name:"lwr_gazebo" component_type:"LWRGazeboComponent" component_package:"rtt-gazebo-lwr-integration" model_name:"kuka-lwr" script:"/vol/toolkit/cogimon-minimal-lwr-nightly/etc/cogimon-scenarios/scenario-wipe-board/script/scn-wipe-board-short.ops"})'
```

<!-- TODO:
* Add link and explanation to CITk distribution / experiment -->

<!-- <p>
  <iframe id="player" type="text/html" width="640" height="390"
      src="http://www.youtube.com/embed/AuTo_6id3J8?enablejsapi=1&origin=http://docs.jetstrap.com/"
        frameborder="0"></iframe>

</p>
 -->