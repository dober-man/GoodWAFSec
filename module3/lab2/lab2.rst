Exercise 3.2: Protection from common exploit vectors
----------------------------------------

Overview
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this exercise you will attack the vulnerable application.  Then apply the blocking policy and observe the results.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Task 1 - Exploring an attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Go to Security --> Event Logs --> Logging profiles

2.  Click Create

  Profile Name: WebGoat
  Applicattion Security: Enabled
  Storage Destination:  Local Storage
  Request Type: All requests

.. NOTE::  Do not use All requests unless for troubleshooting purposes.  Choose Illegal requests, and requests that include staged attack signatures

3.  Go to Local Traffic --> Virtual Servers --> Virtual Server List

4.  Click on the webgoat.f5demo.com_https_vs

.. image:: images/image1_3_2.png

5.  Click on Security and Policies from the top menu

.. image:: images/image2_3_2.png

6.  Make sure to set Application Security Policy to enabled and choose the Blocking_Policy.  Then enabled Log Profile and select the WebGoat profile you created.  Move it to Selected.  Click Update.

.. image:: images/image3_3_2.png

7.  Within Chrome click on the three dots in the upper right and choose New Incognito window

.. image:: images/image4_3_2.png

8.  Click on the Login Page bookmark to get to the WebGoat application

9.  At the username prompt try entering a sequel query for the username and the letter a for the password

::

    or 1='1

.. NOTE:: Did you see anything?  Why do you think you were not blocked?

10.  Return to the BIG-IP Go to Security --> Event Logs --> Application --> requests

11.  You will find an entry there for the login page. (We will examine this further later)

12.  Go to Security --> Application Security --> Policy Building --> Learning and Blocking settings

13.  Click on the carrot next to Attack Signatures and click on the Block check box at the top (this will turn on blocking for all the signatures).  Make sure to click Save and Apply Policy

.. image:: images/image7_3_2.png

14.  On the left menu of the BIG-IP right click on Security and select "Open Link in a new Tab"

15.  Go to the new tab.  Select Security --> Event Logs --> Application --> Requests

16.  Open a New Incognito Window in Chrome

17.  Click the bookmark for Login page

18.  At the username prompt try entering a sequel query for the username and the letter a for the password

::

    or 1='1

.. NOTE:: You should see that you are blocked and received a message with a support ID.
.. image:: images/image8_3_2.png

.. NOTE:: Did the query work?  Why not?

19.  Return to the BIG-IP and the Event Logs tab

20.  In the upper right corner change the auto refresh to 10 seconds

.. image:: images/image9_3_2.png

21.  Click on the log entry for /webgoat/login and examine the request.

22.  Change from Basic to All Details and will see more details regarding the request

.. image:: images/image10_3_2.png

23.  Click on Attack signature detected

.. image:: images/image11_3_2.png

Task 2 - Using ZAP Proxy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Open ZAP Proxy by locating the icon on the top bar |zap_proxy|

2.  Select No, I do not want to persist this session at this moment in time

3.  Enter the following URL in to the URL to Attack field:
::

    http://10.1.10.145/WebGoat

In the upper left corner change the mode to Attack mode and then execute the attack

.. image:: images/image12_3_2.png

4.  Return to the BIG-IP and examine the Event Logs.  You will need to stop the auto refresh by clicking on the countdown

.. image:: images/image13_3_2.png

5.  Take a look at the various attacks conducted by ZAP.  Examine the log entries and what signature prevented the attack from occurring.  You can explore the documentation on the signature as well.

.. |zap_proxy| image:: images/zap_proxy.png
