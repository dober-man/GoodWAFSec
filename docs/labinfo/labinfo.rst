Lab Environment & Topology
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. WARNING:: All work is done from the Linux client/jumphost (client01), which can be accessed via RDP (Windows Remote Desktop) or ssh. No installation or interaction with your local system is required.

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

* `WebGoat 8 <https://github.com/WebGoat/WebGoat/wiki>`_ - WebGoat is a deliberately insecure web application maintained by OWASP designed to teach web application security lessons. You can install and practice with WebGoat. There are other 'goats' such as WebGoat for .Net. In each lesson, users must demonstrate their understanding of a security issue by exploiting a real vulnerability in the WebGoat applications. For example, in one of the lessons the user must use SQL injection to steal fake credit card numbers. The application aims to provide a realistic teaching environment, providing users with hints and code to further explain the lesson.

Why the name "WebGoat"? Developers should not feel bad about not knowing security. Even the best programmers make security errors. What they need is a scapegoat, right? Just blame it on the 'Goat!

.. _lab-topology:

Lab Topology
------------

The network topology implemented for this lab is very simple. The following
components have been included in your lab environment:

-  1 x Ubuntu Linux 16.04 client
-  1 x F5 BIG-IP VE (v13.1.0.5) running ASM and LTM
-  1 x Ubuntu Linux 16.04 server

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
     - https-``ubuntu:f5DEMOs4u!``
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
