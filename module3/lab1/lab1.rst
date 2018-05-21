Exercise 3.1: Blocking Policy
----------------------------------------

Objective
-----------------------

You will explore the blocking policy and settings.  The blocking policy used for this lab will focus on negative security using signatures.

Task 1 - Exploring policy
-----------------------
1.  Within the BIG IP go to Security --> Application Security --> Policy Building --> Learning and Blocking Settings
2.  Click on Blocking Settings...

|lab-3-1-1|

3.  All the blocking and learning checkboxes are unchecked.  Alarming is still turned on for some critical functions. Close the window.

|lab-3-1-2|

4.  Under Policy Building Settings click on the carrot next to Attack Signatures to expand the Attack Signatures options

|lab-3-1-3|

.. NOTE:: For this lab Signature Staging has been disbaled.  In a production environment you should consider using staging to allow yourself mitigation time before new signatures are implemented.

Task 2 - Tuning policy
-----------------------
1.  Under the General Settings you will see various settings for Enforcement, Learning Mode and Learning Speed.  For this lab the policy has been set in to Blocking with Manual Learning and a learning speed of fast.

|lab-3-1-4|

.. NOTE:: Depending on the setting you choose for Learning Mode you may find additional options
|lab-3-1-5|

2.  Under Policy Building Process you will find there are settings for Loosen Policy and Tighten Policy.  While we will not cover these topics in this lab make note of these settings for future learning opportunities.

|lab-3-1-6|

3.  When you have made changes to this page make sure to save the policy and then Apply policy

|lab-3-1-7|  |lab-3-1-8|

.. |lab-3-1-1| image:: images/image1_3_1.png
.. |lab-3-1-2| image:: images/image2-3-1.png
.. |lab-3-1-3| image:: images/image3-3-1.png
.. |lab-3-1-4| image:: images/image4-3-1.png
.. |lab-3-1-5| image:: images/image5-3-1.png
.. |lab-3-1-6| image:: images/image6-3-1.png
.. |lab-3-1-7| image:: images/image7-3-1.png
.. |lab-3-1-8| image:: images/image8-3-1.png
