Exercise 3.3: Server Technologies and Custom Signature Sets
----------------------------------------

Objective
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this exercise we will examine server technologies and custom signature sets.  Server Technologies function allows you to automatically discover server-side frameworks, web servers and operating systems.  This feature helps when the backend technologies are not well known.  The feature can be enabled to auto detect.  You can also add the technologies that you know.  Creating custom signature sets allows you to define what signature groupings work best for your needs.  In this exercise we will explore both.

Task 1 - Server Technologies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Go to **Security > Application Security > Policy Building > Learning and Blocking Settings **

2.  Locate Server Technologies and expand the option.  Click Enable Server Technology Detection

.. image:: images/image1_3_3.png

3.  Make sure to save and Apply Policy.

.. NOTE:: Our policy is currently in manual and we would need to manaully accept all server technologies suggestions to build the server technology signature sets.  If the policy were in automatic learning server technologies would automatically be accepted once the threshold was met.

4.  Click on the diagnal arrow to the left of the Enable Server Technology Dectection.  This will open the Server Technologies configuration which can also be found by going to **Security > Application Security > Policy > Server Technologies**

.. image:: images/image2_3_3.png

5. Click on the drop down box and you will find a list of various server-side technologies to choose from.

.. image:: images/image3_3_3.png

6.  Choose Apache Tomcat from the list.  You will be prompted that Java Servlet/JSP will also be added.  Click okay

.. image:: images/image4_3_3.png

7.  Choose Unix/Linux from the list and click ok.  Make sure to click Save and Apply Policy.

8.  Navigate to **Security > Application Security > Policy Building > Learning and Blocking Settings**

9.  Expand Attack Signatures and you should now see the additional server technology signature sets enabled and in blocking.

.. image:: images/image5_3_3.png

10.  Clear the event logs and try attacking the application again with ZAP and see if you were able to block more attacks.

Task 2 - Create Custom Signature Set
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Go to **Security > Options > Application Security > Attack Signature > Attack Signature Sets**

2.  Click on Create

Fill out the following -
  - Name - ``my_signature_set``
  - Type - ``filter-based``
  - Default Blocking Actions -  ``leave Learn/Alarm/Block checked``
  - Assign To Policy by Default -  ``Uncheck this box``  (in production enabling this feature ensures this signature set is assigned to all newly created policies)
  - Signature Type -  ``Request``
  - Attack Type -  ``All``
  - Systems -  ``Unix/Linux, Apache, Apache Tomcat, Java Servlets/JSP`` Move to the right
  - Accuracy -  ``Equals High``
  - Risk - ``Equals Low``
  - User-defined -  ``All``
  - Update Date -  ``All``

3.  Click on Create.  Now you have a created your own custom signature set of high accuracy signature with server side technologies and low risk.

.. image:: images/image6_3_3.png

4.  Navigate to **Security > Application Security > Policy Building > Learning and Blocking Settings**

5.  Expand Attack Signatures.  Click on Change and uncheck all the signatures currently enabled.  Check the newly created signature set, click Change

.. image:: images/image7_3_3.png

6.  Click Save and Apply policy
