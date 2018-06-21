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

  - Profile Name: WebGoat
  - Applicattion Security: Enabled
  - Storage Destination:  Local Storage
  - Request Type: All requests

.. NOTE::  Do not use All requests unless for troubleshooting purposes.  Choose Illegal requests, and requests that include staged attack signatures

3.  Go to Local Traffic --> Virtual Servers --> Virtual Server List

4.  Click on the webgoat.f5demo.com_https_vs

.. image:: images/image15_3_2.png

5.  Click on Resources

.. image:: images/image16_3_2.png

6.  Click on Manage under the iRules section.  Select _sys_https_redirect from the list of available rules and move it to the Enabled column.  Click finished

.. image:: images/image17_3_2.png

.. NOTE:: This step may not be necessary in a production environment.  This iRule redirects all port 80 traffic to the Virtual Server listening on port 443.

7.  Next click on the webgoat.f5demo.com_https_vs

.. image:: images/image1_3_2.png

8.  Click on Security and Policies from the top menu

.. image:: images/image2_3_2.png

9.  Make sure to set Application Security Policy to enabled and choose the Blocking_Policy.  Then enabled Log Profile and select the WebGoat profile you created.  Move it to Selected.  Click Update.

.. image:: images/image3_3_2.png

10.  Within Chrome click on the three dots in the upper right and choose New Incognito window

.. image:: images/image4_3_2.png

11.  Click on the Login Page bookmark to get to the WebGoat application

12.  At the username prompt try entering a sequel query for the username and the letter a for the password

::

    or 1='1

.. NOTE:: Did you see anything?  Why do you think you were not blocked?

13.  Return to the BIG-IP Go to Security --> Event Logs --> Application --> requests

14.  You will find an entry there for the login page. (We will examine this further later)

15.  Return to the WebGoat application and login with credentials webgoat and f5DEMOs4u!

16.  From the left menu go to Injection Flaws --> SQL Injection and select exercise 7

.. image:: images/image5_3_2.png

17.  In the account name field try an injection attack

::

    %' or 1='1

18.  You will be able to see a wealth of information

.. image:: images/image6_3_2.png

19.  Go to Security --> Application Security --> Policy Building --> Learning and Blocking settings

20.  Click on the carrot next to Attack Signatures and click on the Block check box at the top (this will turn on blocking for all the signatures).  Make sure to click Save and Apply Policy

.. image:: images/image7_3_2.png

21.  On the left menu of the BIG-IP right click on Security and select "Open Link in a new Tab"

22.  Go to the new tab.  Select Security --> Event Logs --> Application --> Requests

23.  Open a New Incognito Window in Chrome

24.  Click the bookmark for Login page

25.  At the username prompt try entering a sequel query for the username and the letter a for the password

::

    or 1='1

.. NOTE:: You should see that you are blocked and received a message with a support ID.
.. image:: images/image8_3_2.png

26.  Repeat steps 16-18

.. NOTE:: Did the query work?  Why not?

27.  Return to the BIG-IP and the Event Logs tab

28.  In the upper right corner change the auto refresh to 10 seconds

.. image:: images/image9_3_2.png

29.  Click on the log entry for /webgoat/login and examine the request.

30.  Change from Basic to All Details and will see more details regarding the request

.. image:: images/image10_3_2.png

31.  Click on Attack signature detected

.. image:: images/image11_3_2.png

Task 2 - Using ZAP Proxy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Open ZAP Proxy by locating the icon on the top bar |zap_proxy|

2.  Select No, I do not want to persist this session at this moment in time

3.  Enter the following URL in to the URL to Attack field:
::

    http://webgoat.f5demo.com/WebGoat

In the upper left corner change the mode to Attack mode and then execute the attack

.. image:: images/image12_3_2.png

4.  Return to the BIG-IP and examine the Event Logs.  You will need to stop the auto refresh by clicking on the countdown

.. image:: images/image13_3_2.png

5.  Take a look at the various attacks conducted by ZAP.  Examine the log entries and what signature prevented the attack from occurring.  You can explore the documentation on the signature as well.

.. |zap_proxy| image:: images/zap_proxy.png

What additional functions can you turn on to prevent some of the other attacks?  How would you turn these on?

.. Bonus::

Go to Security --> Application Security --> Policy Building --> Traffic learning

Explore the Learning suggestions and Traffic Summary page.

Locate the Enforcement Readiness section.

.. image:: images/image14_3_2.png

Click on the numbers.  This will take you to the learning and blocking settings page.  This shows you the settings that could be turned on to better protect your application.

To the left you will find a number of learning suggestions.  As traffic traverses your application these learning suggestions will eventually reach higher percentages.

Click on a learning suggestion to explore.  You will learn how many events have been triggered and give you the option to accept the suggestion, delete the suggestion or ignore.

.. NOTE:: The higher the percentage on the learning score the higher the chance you should accept this suggestion.
