---
layout: default
title: Modeling
section: software/modeling
---
<style>
  h3 {
    margin: 40px 0px 20px 0px;
  }
</style>
<div class="page-header">
  <h1>Open Source Modeling Components</h1>
</div>
The modeling tools we provide are based on the [JetBrains Meta-Programming System](https://www.jetbrains.com/mps). This language workbench supports many relevant aspects of model-driven development in a unified development environment. Information about the DSLs re-used or developed in CogIMon and their concepts will be also provided in the following.


## Modeling environment

We chose JetBrains Meta Programming System ([MPS 3.3.x](http://blog.jetbrains.com/mps/2016/03/mps-3-3-4-has-been-released/)) as modeling environment for our model-driven engineering research. The main advantage of MPS is the ability to extend and combine different languages. This not only fosters re-use, but also makes the whole approach very flexible. Since MPS is a projectional editor, the code is always represented by an [Abstract Syntax Tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) (AST). This enables code verification and constraint checking on the fly, since all operation are directly applied to the AST.

Further advantages and a feature list can be found at [jetbrains.com/mps](https://www.jetbrains.com/mps/).

## Domain-specific languages

The different DSL that are re-used and also those that are developed in CogIMon are presented in the following.

#### Architectural DSLs

* Motion Primitve DSL

   This DSL emerged from the [AMARSi](https://www.amarsi-project.eu/) project and provides a language to create control architectured based on motion primitives.

* Dynamical System DSL

   The dynamical system DSL enables the user to directly create such a system from formulae. It is designed to be used together with the Motion Primitive DSL.

* CCA DSL

   This DSL represents a language that targets the [Compliant Control Architecture](http://docs.cor-lab.org/cca-manual/0.3/html/) (CCA), which is a component architecture developed to enable machine learning and rich motor skills on compliant robot platforms, as part of the [AMARSi](https://www.amarsi-project.eu/) project. Combined with a DSL such as the Motion Primitive DSL, these rich motor skills can be embodied into the non-realtime execution architecture CCA.

* RSB DSL

   Represents and generates the glue-code for the non-realtime middleware.

* Orocos RTT DSL (_in progress_)

   Similar to the CCA DSL, the Orocos language will serve to transform motion as well as force control concepts into the real-time execution runtime [Orocos RTT](http://www.orocos.org/rtt).

* RST DSL

   The [Robotics Systems Types Repository](http://docs.cor-lab.de//rst-manual/trunk/html/index.html) (RST) contains type specifications for robotics and cognitive systems and associated conversion code for different data types and programming languages. These types will be made available inside out modeling environment by this language.

* RCI DSL (_in progress_)

   Analogue to the RST DSL, the [Robot Control Interface](http://docs.cor-lab.org/rci-manual/0.3/html/) (RCI) data-types need to be modelled as well to be used together with the Orocos RTT DSL.

#### Coordination DSLs

* Coordination DSL

   Represents the [SCXML](https://www.w3.org/TR/scxml/) formalism to create state machine artefacts that can be interpreted by the [SCXML-Engine](https://commons.apache.org/proper/commons-scxml/guide/core-engine.html).

* RSB Coordination DSL

   This DSL provides RSB related extensions to the basic Coordination DSL.

* CCA Coordination DSL

   Analogue to the RSB Coordination DSL, the CCA Coordination language provides the necessary extensions to interact with CCA-based components from the coordination point-of-view.

* Orocos Coordination DSL (_in progress_)

   The same that holds for the CCA DSL also holds for this DSL, with the difference that it targets the Orocos RTT components instead of the CCA ones.

#### Visualization DSLs

* GraphML DSL

   It is used to visualize the system architecture in [GraphML](http://graphml.graphdrawing.org/).

* PlantUML DSL

   This is used as well to visualize the system architecture with a different formalism: [PlantUML](http://de.plantuml.com/).
