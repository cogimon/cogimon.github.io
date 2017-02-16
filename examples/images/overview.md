---
layout: default
title: CogIMon Component Library
section: runtime/overview
---
<style>
  h3 {
    margin: 40px 0px 20px 0px;
  }
</style>
<div class="page-header">
  <h1>Runtime Features</h1>
</div>

![CoSimA Abstraction Layers](images/cosima-layers.png "CoSimA Abstraction Layers")

## Execution

The execution architecture, which provides the runtime environment for CogIMon systems consists of two layers: real-time execution and non-real-time execution.

### Real-time execution

For the real-time execution the [Orocos Real-Time Toolkit](http://www.orocos.org/rtt) (RTT) is used. It provides a C++ framework for realtime control systems. Combined with the [Orocos Kinematics and Dynamics Library](http://www.orocos.org/kdl) (KDL) it lays the foundation to implement the motion- / force-control architecture.

### Non-real-time execution

For the non-real-time critical parts of the architecture, either [YARP](http://www.yarp.it/) modules or [RSB](http://docs.cor-lab.org/rsb-manual/trunk/html/) services are used, which are encapsulated in and run as regular Linux processes. These high-level components are integrated with the real-time execution via interprocess communication and the respective OROCOS-RTT transport implementations.
