# 02 VIRTUALIZATION

## Virtualization
* Computer architecture technology
* Multiple VMs are multiplexed within the same hardware
* VMM: Virtual machine monitor
* Objectives:
    * Enhance resource sharing
    * On-the-fly replacement and upgrading of hardware (quick provision)
    * Reducing downtime, improving system efficiency
    * Administrative task can be performed on the VM - a virtual server
* Idea: Separation of hardware from software with intermediate layers

## Physical Machine vs. Virtual Machine

```
Physical Machine            Virtual Machine

Applications                Guest
                            Applications

                            Guest OSs
====================================================
-------------------         -------------------
| OS              |         | VMM             |
-------------------         -------------------
| Hardware        |         | Hardware        |
-------------------         -------------------
```

## Virtualization can be regarded from a software and a hardware perspective
## Levels of Virtualization

```                                     Interfaces for users:
=====================================
 Application level virtualization        API
-------------------------------------
 Library level virtual.                  Application Binary Interface
-------------------------------------
 OS level virtual.                       System commmands, user commands (ISA)
-------------------------------------
 Hardware abstraction level virtual.
-------------------------------------
 Instruction set architecture level
=====================================
```
1. Instruction set level virtualization
    * Emulation of one ISA into another ISA
    * Mapping of opcodes of one machine onto the ones of another machine

2. Hardware abstraction level
    * VMM / hypervisor is placed between the hardware and the host OS
    * Virtualization of a computer's resources (CPUs, memory, I/O devices)
    * Multiple users sharing the hardware concurrently at a time

3. OS level virtualization
    * Create isolated containers on a single physical server
    * Multiple isolated VMs within a single OS kernel
    * Examples:
        * Virtual Execution Environments
        * Virtual Private Systems
        * Containers
        * Single OS Image Virtualizations

4. Library level virtualization
    * Virtualization with library interfaces, i.e. APIs to call interception and remapping
    * Examples: WINE, vCUDA
    * **vCUDA**:
        * CUDA: Programming model and library for GPGPUs
        * vCUDA virtualizes CUDA library and can be invoked from a guest OS

5. Application level virtualization
    * Also called: Process level virtualization
    * Application is virtualized as a VM
    * It acts as a HLL program (high level language) that can be executed on machines of different architectures
    * Example: JVM

## Hardware and OS Level Virtualizations
* Type 2: OS level - e.g. VirtualBox

```
Type 1              Type 2
HW level            OS level

---------------     ---------------
| Guest        |    | Guest        |
| Applications |    | Applications |
---------------     --------------- 
| Guest OSs    |    |**** VMM **** |
---------------     ---------------
=======================================
---------------     ---------------
|**** VMM *****|    | Host OS      |
---------------     ---------------
| Hardware     |    | Hardware     |
---------------     ---------------

```

### Type 1: HW level (Baremetal virtualization) - e.g. VMWare ESXi
* HW abstraction level virtualization
* HV has direct access to HW resources 
* OSs / Guests are installed on top of the HV
* Better performance and stability, but poor HW compatibility
* **Virtualization types**:
    * based on functionality
        * Monolithic HV
        * Microkernel HV
    * based on implementation
        * Full virtualization
            * without binary translation
            * with binary translation
        * Para-virtualization

#### Type 1 Virtualization - Functionality
* Monolithic hypervisor
    * HV implementes virtualization stack
        * Physical memory management
        * Scheduling
        * Drivers, filesystem, application IPC in kernel space
* Microkernel hyperversor
    * HV without loaded drivers
    * Shared virtualization stack with distributed driver model
    * Faster and secure

#### Type 1 Virtualization - Implementation
* Full Virtualization
    * without binary translation:
        * non-critical instruction of applications are executed directly on the hardware
        * critical instructions are trapped and emulated within the hypervisor
    * with binary translation
        * binary translation is applied on trapped critical instruction
        * those instruction are then executed by the VMM
* Para-virtualization
    * [Wikipedia](https://en.wikipedia.org/wiki/Paravirtualization)
    * Provide a software interface to the VMM (not the underlying hardware-software interface!)
    * **Hypercall**: System call to the HV below.
    * HV provides hypercall interface to accomodate critical kernel operations
    * Objective: Improvement of performance by avoiding unnecessary trapping of critical instructions, i.e. to decrease virtualization overhead.

### Type 2: OS level
* Multiple VMs can share a single machine / cluster
* Advantages: Low resource requirements, high scalability --> better speed and performace
* Challenges: 

## Virtualization from HW Perspective

### CPU Virtualization // ToDo!!

### Memory Virtualization
* **Virtual memory**: Memory management technique to avoid system crashes
* Mapping done by VMMs - Two-Stage mapping process
    * `Memory addresses (virtual addresses of VMs) --> RAM (Physical memory) --> Machine memory`
    * Guest OSs can not directly access machine memory
    * **Shadow table**: Page table in VMM

## Virtual Clusters
* Comprised of multiple VMs - Connections of VMs, virtual switches / networks
* VMs are logically connected to distributed servers via a virtual network.
* 1 Virtual Cluster possibly spans across several clusters
* High availability, scalability and performance improvements to an application (real cluster is more expensive)

### Design Aspects
1. Live migration of VMs
2. Live migratino of memory and file systems
3. Dynamic deployment at runtime of VCs

### Dynamic Provisioning
Characteristics:
* A VM runs with a guest OS
* VMs are replicated across multiple servers (for reliability, fault tolerance, disaster recovery)
* Dynamic in/decrease of number of machines
* System continues in case of failure in any physical server which would pull dosn the running VMs

Necessity of effective implementation strategies for a VC, i.e. manual provisioning is not suitable.
```
VC Dynamic Provisioning Strategies
|--- Storage Mechanisms of VM images
|--- VC Deployment
|--- VC Schedulung
|--- Load Balancing
|--- Server Consolidation
```

* **VC Deployment**:
    1. Construct, distribute the whole system of software stack to a physical node
    2. Identify destination node
    3. Switch run time environments from one user's cluster to the other user's cluster
    4. Shutdown / suspension of the cluster upon migration
    * Dealing with large deployment overheads:
        1. Design new migration strategies
        2. Frame an effective load balancer
        3. Increase resource uilization to shorten execution of workloads

* **VM Migration**: Task of moving a VM from one physical hardware environment (host) to another
    * Resource management technique
    * Objective: Load balancing between hosts, green computing (i.e. switching off underutilized hosts), enabling maintenance
    * Stages:
        ```
        0. Pre-migration             > VM running 
        1. Reservation               > normally on Host A
            2. Iterative Pre-copy      > Overhead due to copying
        3. Stop and copy                 > Downtime 
        4. Commitent                     > (VM out of service)
        5. Activation                      > VM running normally on Host B
        ```