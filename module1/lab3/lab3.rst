Lab 1.3: Protocol Compliance
----------------------------------------

Objective
~~~~~~~~~

- Attach the security policy to the appropriate virtual server.

- Validate that the security policy is working correctly.

- Estimated time for completeion **10** **minutes**.

Apply Security Policy
~~~~~~~~~~~~~~~~~~~~~

.. IMPORTANT:: To clearly demonstrate just the protocol compliance protection,
   please remove the previously created DoS profile and  **enable** the ``lab1_webgoat_waf`` Security Policy on the
   ``webgoat.f5demo.com_https_vs`` virtual server!

#. Open a new private browsing window in Firefox and use the bookmark for **WebSwing** to browse open OWASP ZAP.
  |image31|

#. Open a new tab in the Firefox browser and use the bookmark **WebGoat** to browse to the Webgoat application.

#. Browse back to OWASP ZAP tool in the **WebSwing** window. Locate the ``https://webgoat.f5demo.com`` entry in the **Sites**.

#. Right click on the ``https://webgoat.f5demo.com`` entry select the **Open/Resend with Request Editor** option from the flyout window.

#. Select the **Request** tab in the **Manual Request Editor** window.

#. Modify the **Host** header to have no value by removing the ``webgoat.f5demo.com`` value. Press the **Send** button.

 |image32|

#. Observe the server response using the **Response** tabl on the **Manual Request Editor**.

#. Browse to Security > Event Logs > Application > Requests on the BIG-IP GUI. Clear the **Illegal Request** option to view all request recieved by the security policy.

  |image33|

#. Observe the Illegal requests observed by the security policy. What protocol compliance violations were observed by the security policy?

#.
