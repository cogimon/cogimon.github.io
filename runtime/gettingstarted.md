---
layout: default
title: Getting Started
section: runtime/gettingstarted
---

<div class="page-header">

  <h1>Getting Started</h1>

<!--   To get a feel for how the CogiMon modeling toolchain shall work and how you can use it to design control architecture
for hybrid force and motion controllers, check out this intro video: -->
</div>

This document explain installation and test of the CogIMon Simulation Architecture (CoSimA) with support for IIT's COMAN
and the KUKA LWRIV+ robot. Please check back often as we continuously extend this documentation.

### Software Installation

[CoSimA](https://toolkit.cit-ec.uni-bielefeld.de/systems/versions/cogimon-minimal-simulation-distribution-nightly) is
modeled in a [Cognitive Interaction Toolkit (CITk)](https://toolkit.cit-ec.uni-bielefeld.de/) distribution
for easily replication.

It has so far been tested on:

* Ubuntu Trusty LTS 14.04 and LTS 16.04 64 Bit using Gazebo 7 and OROCOS-RTT 2.8

* OS X Yosemite (10.10) / El Capitan (10.11) using Gazebo (LTS 7.1) and OROCOS-RTT 2.8

The following configurations will be supported / tested soon:

* OS X El Capitan (10.11) using Gazebo (LTS 7.1) and OROCOS-RTT 2.9

#### Bootstrap the CITk environment

##### Ubuntu

1. Setup the CITk toolchain on your machine according to the instructions [here](https://toolkit.cit-ec.uni-bielefeld.de/tutorials/bootstrapping)

   **NOTE:** If you are on Ubuntu **Trusty**, you need to source the ROS environment **before** starting the jenkins (./start_jenkins) in the linked
   tutorial. If you are on Ubuntu **Xenial** you **don't** need to source the setup.bash

        source /opt/ros/indigo/setup.bash
        ./start_jenkins

	<!-- ***Note:*** If the download from the stated server is slow, you may also download it from the mirror
	[here](https://www.dropbox.com/sh/1q6w0akfg9fji8t/AAADUDUkU2bCemCEHyoT3-nwa/jenkins.tar.gz?dl=0). -->

1. Clone the CITk recipe repository

		mkdir -p $HOME/citk/dist && cd $HOME/citk/dist
        git clone https://opensource.cit-ec.de/git/citk .

##### OS X

1. Add our homebrew tap, which contains the necessary formulas for the installation.

        brew tap corlab/homebrew-formulas


#### Install Gazebo

##### Ubuntu

_The following commands add Gazebo repositories for binary installation to your system._

1. Setup your computer to accept software from packages.osrfoundation.org.

    ***Note:*** there is a list of [available mirrors](https://bitbucket.org/osrf/gazebo/wiki/gazebo_mirrors) for this
    repository which could improve the download speed.

        sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

2. Setup keys.

        wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

3. Update Package Cache.

        sudo apt-get update

4. Install Gazebo7.

        sudo apt-get install gazebo7
        # For developers that work on top of Gazebo, one extra package
        sudo apt-get install libgazebo7-dev


##### OS X

1. Add the official tap for Gazebo.

        brew tap osrf/simulation


2. Install Gazebo7. _If you're running **Yosemite** you can use the bottled-installation, which is much faster than building from source._

        brew install gazebo7

[In case you experience any problems at this stage, please consult the Gazebo installation  guidelines](http://gazebosim.org/tutorials?tut=install_ubuntu&cat=install)

#### Install CoSimA

##### Ubuntu

1. Install the ```CogIMon Minimal Simulation Distribution-nightly``` distribution as explained
[here for xenial](https://toolkit.cit-ec.uni-bielefeld.de/systems/versions/cogimon-minimal-simulation-distribution-nightly) **or** [here for trusty](https://toolkit.cit-ec.uni-bielefeld.de/systems/versions/cogimon-minimal-simulation-distribution-trusty-nightly).
In particular you need to install the system dependences **first** and call the build generator for the CoSimA distribution.

	***Note:***

	* You may add ```--cache-directory=/tmp``` to the build generator command in order to speed up repeated job generation
	* You need to replace **-u USER and -p YOUR_PASSWORD** with the Jenkins API token that can be retrieved from your Jenkins user profile
	  or the password that you used during the Jenkins bootstrapping.

	A full example of the command line call for **Trusty**:

        $HOME/citk/jenkins/job-configurator --on-error=continue -d $HOME/citk/dist/distributions/cogimon-minimal-trusty-nightly.distribution -m toolkit -u YOUR_USERNAME -p YOUR_PASSWORD -D toolkit.volume=$HOME/citk/systems

    A full example of the command line call for **Xenial**:

        $HOME/citk/jenkins/job-configurator --on-error=continue -d $HOME/citk/dist/distributions/cogimon-minimal-nightly.distribution -m toolkit -u YOUR_USERNAME -p YOUR_PASSWORD -D toolkit.volume=$HOME/citk/systems


2. In your local [Jenkins build server](https://localhost:8080) trigger the ```	distribution-buildflow-cogimon-minimal-nightly``` job (only possible after login).

3. Wait for completion and check that all bullets are blue after the individual build has passed. Jenkins builds and installs the packages to the specified toolkit volume (see command line above).

##### OS X

***Note:*** The **OS X** instructions are still considered WiP. If you experience some problems, feel free to contact us.

    brew install orocos-ocl
    brew install orocos-stdint_typekit
    brew install orocos-typelib
    brew install kdl-typekit
    brew install eigen-typekit
    brew install rsb
    brew install rsb-spread-cpp
    brew install rsb-tools-cl
    brew install rsb-tools-cpp
    brew install rst-converters
    brew install rst-rt
    brew install rtt-core-extensions
    brew install rtt-dot-service
    brew install rtt-gazebo-clock-plugin
    brew install rtt-gazebo-embedded
    brew install rtt-rsb-transport
    brew install rtt-rst-rt-typekit

### System Test

The system can be manually tested in your environment if you follow the following steps.
The commands shown here assume that you execute them in a  terminal using bash.

##### Ubuntu

_For **Ubuntu** you just need to source the following script that sets up all the environmental variables for you._

1. Source the particular script, which you'll find in your _$prefix_ which is _$HOME/citk/systems/cogimon-minimal-[trusty]-nightly_.

        source $HOME/citk/systems/cogimon-minimal-nightly/bin/setup-cogimon-env.sh

        or

        source $HOME/citk/systems/cogimon-minimal-trusty-nightly/bin/setup-cogimon-env.sh

##### OS X

_For **OS X** you need to set the following environmental variables manually._

1. You need to "tell" Orocos where to find the libraries you want to use.

        export RTT_COMPONENT_PATH=/usr/local/lib

2. You need to clone the [model repository](https://github.com/corlab/cogimon-gazebo-models) into your workspace.

        export GAZEBO_MODEL_PATH=< path-to-the-model-repository >

3. You need to "tell" Gazebo where to find the clock plugin for synchronization.

        export GAZEBO_PLUGIN_PATH=/usr/local/lib/< path-to-gazebo-clock-plugin >

#### Start the RSB Server process

    source $HOME/citk/systems/cogimon-minimal-nightly/bin/setup-cogimon-env.sh
    rsb0.16 server

    or

    source $HOME/citk/systems/cogimon-minimal-trusty-nightly/bin/setup-cogimon-env.sh
    rsb0.16 server


You should see some output confirming that the server has started.

#### Start the RTT Deployer console

    source $HOME/citk/systems/cogimon-minimal-nightly/bin/setup-cogimon-env.sh
    deployer-gnulinux

    or

    source $HOME/citk/systems/cogimon-minimal-trusty-nightly/bin/setup-cogimon-env.sh
    source /opt/ros/indigo/setup.bash
    deployer-gnulinux


You should get a shell-style prompt, which allows you to interact with the RTT environment.

#### Load and start the required CoSimA components

Please type within the deployer-console (replace $HOME with the expanded installation prefix = /home/$YOUR_USRNAME/):


    loadService("this","scripting")
    scripting.runScript("$HOME/citk/systems/cogimon-minimal-nightly/etc/cogimon-scenarios/scenario-coman/coman_bring_up_rsb.ops")


You should see quite some output in the deployer that you may ignore for now.

#### Start an RSB Logger process (optional)


    source $HOME/citk/systems/cogimon-minimal-nightly/bin/setup-cogimon-env.sh
    rsb0.16 logger socket:/

    or

    source $HOME/citk/systems/cogimon-minimal-trusty-nightly/bin/setup-cogimon-env.sh
    rsb0.16 logger socket:/


You should see a data stream that sends joint feedback with 100Hz.

#### Start the Gazebo client process


    source $HOME/citk/systems/cogimon-minimal-nightly/bin/setup-cogimon-env.sh
    gzclient

    or

    source $HOME/citk/systems/cogimon-minimal-trusty-nightly/bin/setup-cogimon-env.sh
    gzclient


You should see the robot in the Gazebo front end.
<!--
#### Start the Robot Gui to make the robot move


    source $HOME/citk/systems/cogimon-minimal-nightly/bin/setup-cogimon-env.sh
    rsb-robot-gui1.0

    or

    source $HOME/citk/systems/cogimon-minimal-trusty-nightly/bin/setup-cogimon-env.sh
    rsb-robot-gui1.0


You should see a basic robot gui that allows you to set a joint configuration that is send to the simulated robot once you apply the values. At the end of this step the Gazebo frontend and the robot GUI should look similar to the following screenshot:

![Screenshot of COMAN Simulation and Robot GUI in Gazebo 6](images/cosima.png "COMAN Simulation in Gazebo 6")
-->

<!-- TODO:
* Add link and explanation to CITk distribution / experiment -->

<!-- <p>
  <iframe id="player" type="text/html" width="640" height="390"
      src="http://www.youtube.com/embed/AuTo_6id3J8?enablejsapi=1&origin=http://docs.jetstrap.com/"
        frameborder="0"></iframe>

</p>
 -->
