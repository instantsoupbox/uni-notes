# 02 VIRTUALIZATION I

## Virtualization
* Computer architecture technology
* Multiple VMs are multiplexed within the same hardware
* VMM: Virtual machine monitor (also: hypervisor)
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
```
                                         Interfaces for users:
=====================================
 Application level virtualization        API
-------------------------------------
 Library level virtual.                  Application Binary Interface
-------------------------------------
 OS level virtual.                       System commmands, user commands (ISA)
-------------------------------------
 Hardware abstraction level virtual.
-------------------------------------
 ISA level virtual.
=====================================
```
1. **Instruction set architecture level** virtualization
    * Emulation of one ISA into another ISA
    * Mapping of opcodes of one machine onto the ones of another machine

2. **Hardware abstraction level** virtualization
    * VMM / hypervisor is placed between the hardware and the host OS
    * Virtualization of a computer's resources (CPUs, memory, I/O devices)
    * Multiple users sharing the hardware concurrently at a time

3. **OS level** virtualization
    * Create isolated containers on a single physical server
    * Multiple isolated VMs within a single OS kernel
    * Examples:
        * Virtual Execution Environments
        * Virtual Private Systems
        * Containers
        * Single OS Image Virtualizations

4. **Library level** virtualization
    * Virtualization with library interfaces, i.e. APIs to call interception and remapping
    * Examples: WINE, vCUDA
    * **vCUDA**:
        * CUDA: Programming model and library for GPGPUs
        * vCUDA virtualizes CUDA library and can be invoked from a guest OS

5. **Application level** virtualization
    * Also called: Process level virtualization
    * Application is virtualized as a VM
    * It acts as a HLL program (high level language) that can be executed on machines of different architectures
    * Example: JVM

## Hardware and OS Level Virtualizations    
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
* Both types must execute the machine's instructions in a safe manner.
* **Guest OS**: OS running on top of the hypervisor
* For type 2, **Host OS** is the operating system running on top of the hardware.

### Type 1: HW level (Baremetal virtualization) - e.g. VMWare ESXi
* According to [Tanenbaum](https://en.wikipedia.org/wiki/Modern_Operating_Systems):
    * A **Type 1 hypervisor** is like an operating system, it's the only program running in the most priviledged mode
    * Its job is to support possibly multiple copies of the actual hardware ( - virtual machines), similar to a process a normal operating system runs.
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
* **Para-virtualization**
    * [Wikipedia](https://en.wikipedia.org/wiki/Paravirtualization)
    * **Hypercall**: System call to the VMM below. Equal what a syscall is to a kernel.
    * HV provides hypercall interface to accomodate critical kernel operations.
    * Objective: Improvement of performance by avoiding unnecessary trapping of critical instructions (those are handled now by the HV), i.e. to decrease **virtualization overhead**.

### Type 2: OS level
* According to [Tanenbaum](https://en.wikipedia.org/wiki/Modern_Operating_Systems):
    * **Type 2 hypervisor** is a program that relies on e.g. Windows / Linux to allocate and schedule ressources.
    * It pretends to be a full computer with a CPU and various devices.
* Type 2: OS level - e.g. VirtualBox
* Advantages:
    * Low resource requirements
    * High scalability 
    * Multiple VMs can share a single machine / cluster
    * Better speed and performace
* Challenges: 
    * All OSs in a container must have one type of OS.

## Virtualization from HW Perspective

### CPU Virtualization // ToDo!!

### Memory Virtualization
* **Virtual memory**: Memory management technique to avoid system crashes
* Mapping done by VMMs - Two-Stage mapping process
    * `Memory addresses (virtual addresses of VMs) --> RAM (Physical memory) --> Machine memory`
    * Guest OSs can not directly access machine memory
    * **Shadow page table**: Maps virtual pages used by VM onto the actual pages given by the hypervisor.

# 02 - VIRTUALIZATION II

## Virtual Clusters
* Comprised of multiple connected VMs, virtual switches / networks
* VMs are logically connected to distributed servers via a virtual network.
* 1 Virtual Cluster possibly spans across several clusters.
* Typical practice: Dynamic provisioning of VMs.
* VSs are supposed to solve the **hardware cost problem**.
* Achievements:
    * High availability
    * Scalability and performance improvements to an application (real cluster is more expensive)

### Design Aspects
1. Live migration of VMs
2. Live migration of memory and file systems
3. Dynamic deployment at runtime of VCs

### Dynamic Provisioning (Bereitstellung)
Characteristics:
* A VM runs with a guest OS
* VMs are replicated across multiple servers (for reliability, fault tolerance, disaster recovery)
* Dynamic in/decrease of number of machines
* System continues in case of failure in any physical server which would pull down the running VMs.

Necessity of effective implementation strategies for a VC. Manual provisioning is not suitable.

VC Dynamic Provisioning Strategies
1. Storage Mechanisms of VM images
2. VC Deployment
3. VC Schedulung
4. Load Balancing
5. Server Consolidation


#### VC Deployment
* Deployment considerations to achieve dynamic provisioning: **VC Deployment**
* Steps of Virtual Cluster Deployment:
    1. Construct, distribute the whole system of software stacks to a physical node (usually via templates). 
    2. Identify destination node
    3. Switch run time environments from one user's cluster to the other user's cluster
    4. Shutdown / suspension of the cluster upon migration
    * We can see that the deployment procedure can incur hefty overheads.
* How can we deal with large deployment overheads?
    1. Design new migration strategies
    2. Frame an effective load balancer (autoscaling)
    3. Increase resource uilization to shorten execution of workloads

### VM Migration 
* **VM Migration**: Task of moving a VM from one physical hardware environment (host) to another
    * Resource management technique
    * Objectives: 
        1. Load balancing between hosts
        2. Green computing (i.e. switching off underutilized hosts)
        3. Enabling maintenance
    * Stages:
        ```
        0. Pre-migration             > VM running 
        1. Reservation               > normally on Host A
            2. Iterative Pre-copy      > Overhead due to copying
        3. Stop and copy                 > Downtime 
        4. Commitent                     > (VM out of service)
        5. Activation                      > VM running normally on Host B
        ```
* **Cold migration**: Suspending the OS before transferring it. The VM that is to be migrated has to be shut down. 
* **Hot migration**: Transferring the OS without stopping the OS operations or applications.
* **Copy-on-write (COW) strategy**: 
    * Optimization strategy in accessing data
    * Avoidance of unnecessary copy and writing operations
    * Give pointers (only a reference) to callers of a resource
    * Upon modification, a true copy is created to prevent changes of caller to becoming visible to the others.