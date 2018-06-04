Excercise 3: HTTPS iApp with Policy
----------------------------------------

Overview
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

F5 offers a number of templated installations for various applications.  For generic web based applications you can find the https iapp template.  As an update to this template we have added security functions such as firewall and web application firewall policies that can be deployed with the application.


Task 1 - Deploy iApp with Security
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Go to iApps --> Application Services then click on Create

.. image:: images/image1_4_4.png

2.  Give the application a name

3.  In the drop down box for template choose f5.htt.v1.3.Orc3

.. image:: images/image2_4_4.png

.. NOTE::  This template has been imported for this lab.  You will find this template at F5 Downloads.  Follow this article on how to download: https://support.f5.com/csp/article/K98001873  The deployment guide can be found here:  https://www.f5.com/pdf/deployment-guides/iapp-http-dg.pdf

4.  New information appears below that will allow you to configure an application with web application security.  In the network section answer Yes, use the new profiles.

.. image:: images/image4_4_4.png

5.  In the SSL Encryption section select Terminate SSL from clients, plaintext to servers (SSL Offload)

.. image:: images/image5_4_4.png

6.  In the Application Security Manager section select Yes, use ASM and create a new ASM policy.  Also select the WebGoat logging profiles

.. image:: images/image6_4_4.png

7.  In the Virtual Server and Pool section give the IP Address, an FQDN and select the webgoat_pool

.. image:: images/image7_4_4.png

8.  Click finished and have patience while the application objects are built

.. image:: images/image8_4_4.png

9.  Go to Security --> Application

9.  Open a new icognito window in Chrome and enter app1.f5demo.com/WebGoat in the address bar.  When you get the SSL warning click Advanced and Proceed

.. image:: images/image9_4_4.png

10.  Login with webgoat and f5DEMOs4u!

11.  You can try surfing around the application.  Try an injection attack.

12.  Return to the BIG IP.  Go to Security --> Applicaiton Security --> Policy Building --> Traffic Learning

Do you see learning suggestions?

13.  Go to Security --> Application Security --> Policy Building --> Learning and Blocking Settings

14.  Click the carrot by Attack Signatures then Change at the far right to add more signature.

15.  Choose the High Accurary Signatures and SQL injection

.. image:: images/image11_4_4.png

16.  Click Save and Apply Policy


Task 2 - Attack Application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
