# Day 1

# Before Virtualization was available,how we can install two or more Operating System in the same machine
<pre>
- Boot Loaders - is a system utility that helps us install many OS ( typically 2 to 4  per laptop/desktop )
- Only 1 OS can be active at any point of time
- We can't run more more 1 OS, it is impossible
- Examples of Bootloaders
  - LILO (Linux Loader)
    - is a very old boot loader that was used in older linux distributions
    - right now not used
  - GRUB 
    - is used by every Linux Distributions
    - supports booting Windows, Mac, Unix & Linux
    - is opensource
  - BootCamp
    - is a boot loader used to support windows on a Mac machine
</pre>  

## Processor
<pre>
- Processors comes in 2 types of Packaging
  - SCM 
    - Single Chip Module
    - one IC will have just one Processor
  - MCM 
    - Multiple Chip Module
    - one IC will have many Processors
- Each Processor may support many CPU Cores
  - 256 cores per Processor
  - 512 cores per Processor
  - 128 cores per processor
  - 64 cores per processor
- Server grade motherboards will support many Processor sockets
- assume a server motherboard support 4 Processor Sockets
- in each of those Processor Socket, if we install a MCM Processor with 4 Processor per IC
- in total how many Processor in the motherboard 16 Processors
- let's assume each Processor supporting 128 cores
- total cpu cores 128 x 16 = 2048 Physical CPU Cores
- For Hypervisors, they look for logical/virtual core
  - each Physical core supports 2 to 4 virtual cores, in a normal processor each physical core supports 2 logical/virtual core
  - how many logical cores Hypervisors will see 2048 x 2 = 4096 Logical/Virtual Cores

</pre>  

## Hypervisor Overview
<pre>
- is virtualization technology
- virtualization allows us running many OS paralelly side by side in the same laptop/desktop/workstation/server
- there are types of Hypervisor
  1. Bare Metal Hypervisor aka Type1 
     - Used in Servers/Workstations
     - Examples
       - VMWare ESXi/VMWare vSphere
       - KVM ( works in all Linux distributions )
  2. Type 2 
     - Used in Laptops/Desktops/Workstations
     - Examples
       - VMWare Wokstation ( Windows & Linux )
       - VMWare Fusion ( Mac OS-X )
       - Hyper-V ( Windows )
       - Parallels( Mac OS-X )
- Each Virtual Machines that runs on top of Hypervisor, will allocated with dedicated
  - CPU Cores ( Logical or Virtual Cores )
  - RAM 
  - Storage
  - Virtual Network Card
  - Virtual Graphics Card
- Each Virtual Machine(VM - aka Guest OS) represents one fully functional Operating System
- This type of virtualization is called Heavy weight Virtualization, the reason being each VM requires dedicated hardware resources
</pre>

## Container Technology
<pre>
- an application virtualization technology
- each container represents one application process
- containers are not Operating system
- container may resemble like a VM or an OS in certain ways but they are just application process
- this type of virtualization is called lite-weight virtuatlization
- all the containers running on an OS shares the hardware resources available to the underlying OS
</pre>

## Info - Container Orchestration Platform Overview
