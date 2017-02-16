---
layout: default
title: Modeling Workbench
section: modeling/workbench
---
<style>
  h3 {
    margin: 40px 0px 20px 0px;
  }
</style>
<div class="page-header">
  <h1>Open Source Modeling Workbench</h1>
</div>

The modeling tools we provide are based on the [JetBrains Meta-Programming System](https://www.jetbrains.com/mps). This language workbench supports many relevant aspects of model-driven development in a unified development environment. Information about the DSLs re-used or developed in CogIMon and their concepts will be also provided in the following.


## Modeling environment

We chose JetBrains Meta Programming System ([MPS 3.4.x](https://blog.jetbrains.com/mps/2017/01/mps-3-4-3-released/)) as modeling environment for our model-driven engineering research. The main advantage of MPS is the ability to extend and combine different languages. This not only fosters re-use, but also makes the whole approach very flexible. Since MPS is a projectional editor, the code is always represented by an [Abstract Syntax Tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) (AST). This enables code verification and constraint checking on the fly, since all operation are directly applied to the AST.

Further advantages and a feature list can be found at [jetbrains.com/mps](https://www.jetbrains.com/mps/).
