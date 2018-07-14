Exercise 2.1: Protocol Compliance
----------------------------------------

Objective
~~~~~~~~~

- Attach the security policy to the appropriate virtual server.

- Validate that the security policy is working correctly.

- Estimated time for completion **10** **minutes**.

Apply Security Policy
~~~~~~~~~~~~~~~~~~~~~

.. IMPORTANT:: To clearly demonstrate just the protocol compliance protection, on the ``webgoat.f5demo.com_https_vs`` virtual server

1. **Remove** the previously created DoS profile and bot logging profile.
2. **Enable** the ``lab1_webgoat_waf`` Security Policy

Your virtual should look like this

.. image:: images/image1.PNG

#. Browse to **Applications > Other > OWASP ZAP** from the Linux OS desktop tool bar or click the springboard icon in the quick launch bar at the top of the screen.

#. Upon initialization, OWASP ZAP will ask if you would like to require session persistence. Select the ``No, I do not want to persist this session at this moment in time.`` option then click **Start**.

.. image:: images/image2_1_1.PNG

3. Launch the **Firefox** browser from within ZAP using the using the **Launch Browser** button while **Firefox** is selected.

.. image:: images/image2_1_2.PNG

4. Return to the OWASP ZAP tool. Locate the ``https://webgoat.f5demo.com`` entry under **Sites**.

.. image:: images/image2_1_3.PNG

5. Right click on the ``https://webgoat.f5demo.com`` entry select the **Open/Resend with Request Editor** option from the flyout window.

.. image:: images/image2_1_4.PNG

6. Select the **Request** tab in the **Manual Request Editor** window.

7. Modify the **Host** header to have no value by removing the ``webgoat.f5demo.com`` value. Press the **Send** button.

.. image:: images/image2_1_5.PNG

8. Observe the server response using the **Response** tab of the **Manual Request Editor**.

9. Browse to **Security > Event Logs > Application > Requests** on the BIG-IP GUI. Clear the **Illegal Request** option to view all request recieved by the security policy.



10. Observe the Illegal requests observed by the security policy. What protocol compliance violations were observed by the security policy?
