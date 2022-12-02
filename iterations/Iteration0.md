# Iteration 0
The goal is to establish a conceptual architecture for offline data processing.

# Drivers
* Objectives and Model from **R01**
    * Objectives: Distributed Processing (slide 4)
    * Boxes Model: Process triggering, computation definition, data processing, algorithmic implementation (slide 5)
* Derived system requirements from **R02**:
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
| Structure the system using the "layers" pattern. **R00** | Regardless of the interactions and coupling between different parts of the system, there is a need to develop and evolve them independently. The layers pattern enforces a clear separation of concerns in the software’s architecture. |
| Strict or Relaxed layers.| We will default to strict layering which means only adjacent layers can communicate. This may need to be relaxed in the future.|
| Template Method Pattern | Intent is to define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure. **R03**|
| Dependency Inversion Principle | High-level components should not depend on low-level components; rather, both should depend on abstractions. **R04** |

# Architecture Elements
| Element     | Responsibilities |
| ----------- | ---------------- |
| Application Layer | Select and invoke a solver framework specialization.|
| Scientific Software Layer **R00** | Provide a set of solver framework specializations each of which utilize the CASA API, include a parallelization request, and depend on infrastructure abstractions.|
| Infrastructure Layer | Communicate actual parallelization availability and execute solver framework specializations.| 

# Views
## Layered Architecture

<p align="center">
  <img src="https://github.com/whiteheaddmark/ngData-Architecture/blob/master/images/ARDGLayers.png?raw=true">
</p>

<div align="center">Figure 1 Generic REST services backed by a database, a file system, or a non-NRAO resource.</div>
</br>

## Template Method Pattern

# Analysis


