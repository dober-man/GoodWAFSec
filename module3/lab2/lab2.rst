Exercise 3.2: Protection from common exploit vectors
===========================

Overview
-----------------------

In this exercise you will attack the vulnerable application.  Then apply the blocking policy and observe the results.

-----------------------

Task 1 - Exploring an attack
-----------------------

1.  Go to Local Traffic --> Virtual Servers --> Virtual Server List

2.  Click on the asm_vs

|lab-3-2-1|

3.  Click on Security and Policies from the top menu

|lab-3-2-2|

4.  Make sure to set Application Security Policy to Disabled and that a logging profile has been set

|lab-3-2-3|

5.  Within Chrome click on the three dots in the upper right and choose New Incognito window

|lab-3-2-4|

6.  Click on the Login Page bookmark to get to the WebGoat application

7.  At the username prompt try entering a sequel query for the username and the letter a for the password

::

    %' or 1='1

.. NOTE:: Did you see anything?  Why do you think you were not blocked?

8.  Login with the credentials webgoat and f5DEMOs4u!

9.  Click on Injection Flaws from the left menu and then on SQL Injection

10.  Within the Account Name field try the injection attack again
::

    %' or 1='1

|lab-3-2-5|

.. NOTE:: You will see that you are able to access the database and gather a wealth of useful information

|lab-3-2-6|

11.  Return to the BIG IP and access the Virtual Server asm_vs.  Click on Security/Policies

12.  Within the Application Security Policy section choose Enabled and Blocking_Policy from the drop down menus.  Then click update

|lab-3-2-7|

13.  On the left menu of the BIG IP right click on Security and select "Open Link in a new Tab"

14.  Go to the new tab.  Select Event Logs --> Application --> Requests

15.  Open a New Incognito Window in Chrome

16.  Click the bookmark for Login page

17.  At the username prompt try entering a sequel query for the username and the letter a for the password

::

    %' or 1='1

.. NOTE:: You should see that you are blocked and received a message with a support ID.
|lab-3-2-8|

18.  Use the back button and repeat steps 8-10

.. NOTE:: Did the query work?  Why not?

19.  Return to the BIG IP and the Event Logs tab

20.  In the upper right corner change the auto refresh to 10 seconds

|lab-3-2-9|

21.  Click on the log entry for /webgoat/login and examine the request.

22.  Change from Basic to All Details and will see more details regarding the request

|lab-3-2-10|

23.  Click on Attack signature detected

|lab-3-2-11|

Task 2 - Using ZAP Proxy
-----------------------

1.  Open ZAP Proxy by locating the icon on the top bar |zap_proxy|

2.  Select No, I do not want to persist this session at this moment in time

3.  Enter the following URL in to the URL to Attack field:
::

    http://10.1.10.145/WebGoat

In the upper left corner change the mode to Attack mode and then execute the attack

|lab-3-2-12|

4.  Return to the BIG IP and examine the Event Logs.  You will need to stop the auto refresh by clicking on the countdown

|lab-3-2-13|



.. |lab-3-2-1| image:: images/image1_3_2.png
.. |lab-3-2-1| image:: images/image2_3_2.png
.. |lab-3-2-2| image:: images/image2_3_2.png
.. |lab-3-2-3| image:: images/image3-3-2.png
.. |lab-3-2-4| image:: images/image4-3-2.png
.. |lab-3-2-5| image:: images/image5-3-2.png
.. |lab-3-2-6| image:: images/image6-3-2.png
.. |lab-3-2-7| image:: images/image7-3-2.png
.. |lab-3-2-8| image:: images/image8-3-2.png
.. |lab-3-2-9| image:: images/image9-3-2.png
.. |lab-3-2-10| image:: images/image10-3-2.png
.. |lab-3-2-11| image:: images/image11-3-2.png
.. |zap_proxy| image:: images/zap_proxy.png
.. |lab-3-2-12| image:: images/image12-3-2.png
.. |lab-3-2-13| image:: images/image13-3-2.png
