Exercise 3.3: Troubleshooting
----------------------------------------

Objective
-----------------------

In this exercise we will examine the response pages, event logs and briefly look at utilizing HTTP capture tools

Task 2 - Response Pages
-----------------------

1.  Go to Security --> Application Security --> Policy --> Response pages

|lab-3-3-1|

2.  Within this area you can add various response pages for different request.  These pages can be modified by editing the response body. On the Default change the Response Type to "Custome Response".  This will open up the Response Body to editing.

|lab-3-3-2|

3.  Edit the Response as follows:

::

    <html><head><title>Request Rejected</title></head><body>You have requested a site that is unavailable. Please contact customer service at 888-555-1212 and supply the following information:<br><br>Support ID: <%TS.request.ID()%><br><br><a href='javascript:history.back();'>[Go Back]</a></body></html>

4.  Click on the Show button

|lab-3-3-3|

5.  Click Save and Apply Policy.  And click OK.

6.  Open a New Incognito Window in Chrome

7.  Try entering a sql injection at the login prompt

::

    %' or 1='1

You should have received a reponse page that you customized.  Make note of the Support ID before moving on to the next task.

|lab-3-3-4|

.. NOTE:: Explore the other response pages.  Observe that AJAX reponse pages are disabled by default.

Task 3 - Event logs
-----------------------

1.  On the BIG IP return to the Security --> Event Log --> Application --> requests

2.  Click on the magnifying glass and that will open the log filter.  From here you can enter the Support ID you received from the preceeding task and select Apply Filter.

|lab-3-3-8|

2.  Select an entry in the event logs.  At the top box you will find button to open the request in a separate tab

|lab-3-3-5|

3.  Click on Attack signature detected

|lab-3-3-6|

Observe the detected attack, the expected parameter, and what the applied blocking settings were.  Also note that the signature used to block this attack has been identified.  By clicking on the "i" next to the name you can get further information on the signature as well as a link to other documentation.

|lab-3-3-7|

4.  Examine the HTTP Header information

|lab-3-3-9|

5.  Observe the Source IP, Accept Status and Support ID.

|lab-3-3-10|

6.  Clocse this tab and return to the BIG IP Event Logs.  Open the filter again and click on Not Blocked.  Apply Filter

|lab-3-3-11|

7.  Locate an entry with a 500 or 405 response code

|lab-3-3-11|

8.  Pop it out in to a new tab.  Why was this illegal action not blocked?  What was the attack type?  What was the violation rating?


.. |lab-3-3-1| image:: images/image1_3_3.png
.. |lab-3-3-2| image:: images/image2-3-3.png
.. |lab-3-3-3| image:: images/image3-3-3.png
.. |lab-3-3-4| image:: images/image4-3-3.png
.. |lab-3-3-5| image:: images/image5-3-3.png
.. |lab-3-3-6| image:: images/image6-3-3.png
.. |lab-3-3-7| image:: images/image7-3-3.png
.. |lab-3-3-8| image:: images/image8-3-3.png
.. |lab-3-3-9| image:: images/image9-3-3.png
.. |lab-3-3-10| image:: images/image10-3-3.png
.. |lab-3-3-11| image:: images/image11-3-3.png
