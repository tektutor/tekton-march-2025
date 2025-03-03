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
       - Hypervisor ( Windows )
- Each Virtual Machines that runs on top of Hypervisor, will allocated with dedicated
  - CPU Cores ( Logical or Virtual Cores )
  - RAM 
  - Storage
  - Virtual Network Card
  - Virtual Graphics Card
- Each Virtual Machine(VM - aka Guest OS) represents one fully functional Operating System
- This type of virtualization is called Heavy weight Virtualization, the reason being each VM requires dedicated hardware resources
</pre>

## Info - Container Orchestration Platform Overview
