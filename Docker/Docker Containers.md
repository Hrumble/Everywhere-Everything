
## How is a container different from a virtual machine?

First understand **how a virtual machine works**:
 
A virtual machine runs via what's called a **hypervisor**, which is a fancy way of saying a *tool to manage virtual machines*. A virtual machine, provided correct allocation of hardware and resources, runs an entirely different OS, on top of another OS.

**This comes with drawbacks:**
- Extremely slow to start (*it's running an entire OS can you blame it*)
- Resource intensive (*You need to allocate a part of your hardware to actually run the **VM***).
- Cannot run a single purpose VM (*You can't set up a low resource cost **VM** to simply run a program, you need to run an entire **OS***)

Of course, you guessed it, **Docker Containers** are a godsend remedy to those problems.

A **Docker Container** does not run an entire OS, rather, it runs a set of specified apps inside a virtual environment. No resource allocation is needed, as each container uses what it needs. They **use the OS of the host**, are **extremely lightweight** and **start quickly**.


 