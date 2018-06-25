Lab Environment & Topology
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. WARNING:: All work is done from the Linux client/jumphost (client01), which can be access via RDP (Windows Remote Desktop) or ssh. No installation or interaction with your local system is required.

All pre-built environments implement the Lab Topology shown below.  Please
review the topology first, then find the section matching the lab environment
you are using for connection instructions.

Environment
-----------

Linux client (client01):

* Web Attack Tools:

 * `Goldeneye <https://github.com/jseidl/GoldenEye>`_ - HTTP DOS Tool
 * `Metasploit <https://www.metasploit.com/>`_ - Pen testing framework
 * `nmap/nping <https://nmap.org/>`_ - Network mapper
 * `Slowhttptest <https://github.com/shekyan/slowhttptest>`_ - HTTP DOS Tool
 * `wapiti <http://wapiti.sourceforge.net/>`_ - web application auditor
 * `w3af <http://w3af.org/>`_ - web application auditor

* Api Tools:

 * `Ansible <https://www.ansible.com/>`_ - Automation platform
 * `curl <https://curl.haxx.se/>`_ - command line webclient, will be used to interact with the iControl Rest API
 * `Postman <https://www.getpostman.com/>`_ - Graphical based Restful Client, will be used to interact with the iControl Rest API
 * `python <https://www.python.org/>`_ - general programming language used to interact with the iControl Rest API

Linux server (server01):

* `WebGoat 8 <https://github.com/WebGoat/WebGoat/wiki>`_ - deliberately insecure application

.. _lab-topology:

Lab Topology
------------

The network topology implemented for this lab is very simple. The following
components have been included in your lab environment:

-  1 x Ubuntu Linux 16.04 client - friendly name: client01
-  1 x F5 BIG-IP VE (v13.1.0.5) running ASM and LTM - friendly name: bigip01
-  1 x Ubuntu Linux 16.04 server - friendly name: server01

.. nwdiag:: images/Agility2018LabDiagram.png
   :width: 800
   :caption: Lab Topology
   :name: lab-topology-diagram
   :scale: 110%

The following table lists VLANS, IP Addresses and Credentials for all
components:

.. list-table::
   :widths: 15 15 15 15 15
   :header-rows: 1
   :stub-columns: 1


   * - **Component**
     - **mgmtnet IP**
     - **clientnet IP**
     - **servernet IP**
     - **Credentials**
   * - Linux Client (client01)
     - 10.1.1.51
     - 10.1.10.51
     - N/A
     - https-``f5student:f5DEMOs4u!``
   * - Bigip (bigip01)
     - 10.1.1.245
     - 10.1.10.245
     - 10.1.20.245
     - https - ``admin:f5DEMOs4u!`` ssh - ``f5student:f5DEMOs4u!``
   * - Linux Server (server01)
     - 10.1.1.252
     - N/A
     - 10.1.20.252
     - ssh - ``f5student:f5DEMOs4u!``

A graphical representation of the lab:


|labDiagram|

.. |labDiagram| image:: images/Agility2018LabDiagram.png

