Lab Topology & Tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. WARNING:: All work for this lab will be performed exclusively from the Windows
   Jumphost. No installation or interaction with your local system is
   required.

All pre-built environments implement the Lab Topology shown below.  Please
review the topology first, then find the section matching the lab environment
you are using for connection instructions.

.. _lab-topology:

Lab Topology
------------

The network topology implemented for this lab is very simple. Since the
focus of the lab is Control Plane programmability rather than Data Plane
traffic flow we can keep the data plane fairly simple. The following
components have been included in your lab environment:

-  1 x F5 BIG-IP VE (v13.1.0.5)
-  1 x Kali Server
-  1 x Windows Jumpbox
-  1 x Vulnerable Web App (Hackazon)

.. nwdiag:: labtopology.diag
   :width: 800
   :caption: Lab Topology
   :name: lab-topology-diagram
   :scale: 110%

The following table lists VLANS, IP Addresses and Credentials for all
components:

.. list-table::
   :widths: 15 30 30 30
   :header-rows: 1
   :stub-columns: 1


   * - **Component**
     - **Management IP**
     - **VLAN/IP Address(es)**
     - **Credentials**
   * - Linux Jumphost - ubuntuf
     - 10.1.1.20
     - **Internal:** 10.1.10.20

       **External:** 10.1.20.20
     - ``ubuntu/supernetops``
   * - BIG-IP A
     - 10.1.1.10
     - **Internal:** 10.1.10.10

       **Internal (Float):** 10.1.10.13

       **External:** 10.1.20.10

       **External (VIPs):** 10.1.20.120-130

     - ``admin/admin``

       ``root/default``
   * - BIG-IP B
     - 10.1.1.11
     - **Internal:** 10.1.10.11

       **Internal (Float):** 10.1.10.13

       **External:** 10.1.20.11

       **External (VIPs):** 10.1.20.120-130

     - ``admin/admin``

       ``root/default``
   * - iWorkflow
     - 10.1.1.12
     - N/A
     - ``admin/admin``

       ``root/default``
   * - Linux Server
     - 10.1.1.15
     - **Internal:** 10.1.10.100-103
     - ``root/default``

Lab Tools
----------------

F5 Virtual Edition (ASM)
^^^^^^^^^^^^^^^^^^^^^^^^

F5's latest version of the BIG-IP application proxy running as a virtual appliance.  The BIG-IP instance we are using today will be provisioned for both Local Traffic Management as well as Web Application Firewall, to allow us to detect and respond to various application attacks.


Kali Linux
^^^^^^^^^^

A Debian based Linux distro designed for digital forensics and security testing, comes pre-loaded with many of the tools we will be using in today's lab.  Reason's to give it a try:

*  Single user, root access by design
*  Network Services disabled by default
*  Custom Linux Kernel
*  A minimal and trusted set of repositories


Hackazon
^^^^^^^^

Our vulnerable web application.  Designed with the best of intentions, but with more holes than a sieve.



Lab Logins
-----------

.. toctree::
   :maxdepth: 1
   :glob:
