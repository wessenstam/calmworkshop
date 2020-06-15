********************************
Introduction to Nutanix Calm-DSL
********************************

This lab is intended to gain hands-on experience based on the topics covered in the calm-dsl introduction workshop.


Agenda
++++++

- What is calm-dsl?
- Setting up the environment
- Basic blueprint using CALM-DSL
- Task: LAMP Blueprint in CALM
- Advanced topics


What is calm-dsl?
+++++++++++++++++

A domain-specific language **(DSL)** is a computer language specialized to a particular application domain.

**Calm-DSL** describes a simpler Python3 based DSL for writing Calm blueprints. As Calm uses Services, Packages, Substrates, Deployments and Application Profiles as building blocks for a Blueprint, these entities can be defined as python classes. Their attributes can be specified as class attributes and actions on those entities (procedural runbooks) can be defined neatly as class methods. Calm blueprint DSL can also accept appropriate native data formats such as YAML and JSON, allowing the reuse and leveraging that work into the larger application lifecycle context of a Calm blueprint.

.. figure:: images/what-is-calm-dsl.png


Pre-requisites
++++++++++++++

Before starting this workshop we assume you have the following skills and tools:

* **Calm:** Good understanding of Calm architecture and building blocks of a blueprint
* **Python3:** Basic knowledge is required. Variables, classes, functions and python syntax
* **Git:** Basic git commands
* **Docker:** Basic knowledge on running an image
* **IDE:** IDE that supports python. Suggested Visual Studio Code with Docker and Python extensions

Initial Setup
+++++++++++++

- Take note of the *Passwords* being used on the HPOC.

Environment Details
+++++++++++++++++++

Nutanix Workshops are intended to be run in the Nutanix Hosted POC environment. You will require Prism Central with Calm enabled.
