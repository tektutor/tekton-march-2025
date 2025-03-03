# Day 1

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
