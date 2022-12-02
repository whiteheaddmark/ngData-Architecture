# Iteration 0
The goal is to establish a conceptual architecture for offline data processing.

# Drivers
* Objectives and Model from (**R01**)
    * Objectives: Distributed Processing (slide 4)
    * Boxes Model: Process triggering, computation definition, data processing, algorithmic implementation (slide 5)
* Derived system requirements from (**R02**):
    * A system architecture for a layered and modular design that scales with continuous evolution of the computational
needs, computing hardware and algorithms evaluation, and permits integration of third-party libraries without requiring
they be rewritten.
    * A framework for process-level parallelization. This includes support for both, a network for loosely-coupled distributed
nodes and a more tightly coupled cluster of computers with shared resources.
    * A prototype to evaluate interfacing system-level frameworks with algorithm implementations in external libraries.
    * A prototype to evaluate a software structure with access to multiple layers of programming interfaces.

# Design Concepts
| Design Decision | Rationale |
| --------------- | --------- |
| Structure the system using the "layers" pattern. (**R00**) | Regardless of the interactions and coupling between different parts of the system, there is a need to develop and evolve them independently. The layers pattern enforces a clear separation of concerns in the software’s architecture. |
| Strict or Relaxed layers.| We will default to strict layering which means only adjacent layers can communicate. This may need to be relaxed in the future.|
| Template Method Pattern | Intent is to define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure. (**R03**) |
| Dependency Inversion Principle | High-level components should not depend on low-level components; rather, both should depend on abstractions. (**R04**) |

# Architecture Elements
| Element     | Responsibilities |
| ----------- | ---------------- |
| Application Layer | Select and invoke a solver framework specialization.|
| Scientific Software Layer (**R00**) | Provide a set of solver framework specializations each of which utilize the CASA API, include a parallelization request, and depend on infrastructure abstractions.|
| Infrastructure Layer | Communicate actual parallelization availability and execute solver framework specializations.| 

# Views
## Layered Architecture
This isn't a new idea. ARDG has already been using it.
<p align="center">
  <img src="https://github.com/whiteheaddmark/ngData-Architecture/blob/master/images/ARDGLayers.png?raw=true" width="800" height="800">
</p>

<div align="center">Figure 1 ARDG layered architecture view.</div>
</br>
In the context of ngData, we propose the following layers:
<p align="center">
  <img src="https://github.com/whiteheaddmark/ngData-Architecture/blob/master/images/ngData-Layers.png?raw=true">
</p>

<div align="center">Figure 2 ngData layers view.</div>
</br>
Applying the Dependency Inversion Principle yields:
<p align="center">
  <img src="https://github.com/whiteheaddmark/ngData-Architecture/blob/master/images/ngData-Layers-with-DIP.png?raw=true">
</p>

<div align="center">Figure 3 ngData layers with dependency inversion view.</div>
</br>

## Template Method Pattern
Here is the general idea behind the Template Method design pattern (**R05**):
<p align="center">
  <img src="https://github.com/whiteheaddmark/ngData-Architecture/blob/master/images/Template-Method-Structure.png?raw=true">
</p>

<div align="center">Figure 4 Template Method structure view.</div>
</br>
This translates into the following code snippet:
<p align="center">
  <img src="https://github.com/whiteheaddmark/ngData-Architecture/blob/master/images/Template-Method-Code-Snippet.png?raw=true">
</p>

<div align="center">Figure 5 Template Method code snippet.</div>
</br>
The key idea is to translate the general structure of the solver framework into a template method where the framework components comprise the steps in the framework algorithm. The CASA API would provide a set of stateless functions representing primitive operations.
<p align="center">
  <img src="https://github.com/whiteheaddmark/ngData-Architecture/blob/master/images/Solver-Specialization.png?raw=true">
</p>

<div align="center">Figure 6 Solver specialization view.</div>
</br>
There would be a finite (and hopefully small) set of solver specializations to cover scientific use cases. 

Each solver would be encoded with parallelization requirements which could be compared to actual resource availability provided by the infrastructure layer.

# Analysis
This model...:
* resolves the executive control problem via the Application Layer.
* avoids concrete coupling between layers.
* provides open-ended support for algorithms based on the general solver framework.
* supports combining dependency inversion with other principles like dependency injection and inversion control to support the derived system requirements, if needed.

Potential problems include:
* the Template Method concepts may not map perfectly to solver framework and CASA API concepts - more work is needed here - but the general idea is the kind of approach that addresses the derived system requirements
* proliferation of primitive operations that need overriding could become tedious and difficult to maintain
* large set of solver specializations could be difficult to maintain
* could be difficult to create an infrastructure interface that suits Dask, Workspaces, TBD...
* it may be desirable to use a functional programming paradigm instead of an OO-based paradigm

