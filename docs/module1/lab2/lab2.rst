Exercise 1.3: Proactive Bot Defense
----------------------------------------

Objective


-  Create a DoS profile

-  Enable proactive bot defense

-  Apply the policy to the appropriate virtual server

-  Validate that the policy is working as expected

-  Estimated time for completion: **20** **minutes**

Create Policy
~~~~~~~~~~~~~


.. IMPORTANT:: To clearly demonstrate just the Bot Defense profile,
   please **disable** the Application Security Policy from the
   ``webgoat.f5demo.com_https_vs`` virtual server!

#. Open the **Terminal** application.

#. Run the following curl command to verify the site is loading without
   issue from this headless browser. If the curl command is not
   successful (you are getting a “request rejected” error page), please
   let an instructor know.

   ``curl –k https://webgoat.f5demo.com/WebGoat/login | more``

   .. image:: images/image1_3_1.PNG

#. On the Main tab, click **Security > DoS Protection > DoS Profiles**.
   The DoS Profiles screen opens.

   .. image:: images/image1_3_2.PNG

#. Click on the **Create** button.

#. Name the policy ``webgoat_DoS`` and click **Finished** to
   complete the creation of this DoS profile.

   .. image:: images/image1_3_3.PNG

Configure Policy
~~~~~~~~~~~~~~~~


#. **Click** the newly created ``webgoat_DoS`` profile listed under the
   **Security > Dos Protection > DoS Profiles** list.

#. The profile’s properties menu will be displayed initially. **Click**
   on the **Application Security** tab at the top of this menu to
   begin configuring the policy.

   .. image:: images/image1_3_4.PNG

#. Under the **Application Security** tab >> General Settings
   click the **Edit** link on the right-hand side of General Settings
   box and then check the ``Enabled`` check box for **Application
   Security** to enable the DoS profile and allow additional settings
   to be configured.

   .. image:: images/image1_3_5.PNG

#. Select **Proactive Bot Defense** under the list of **Application
   Security** options for this DoS profile.

#. Click the **Edit** link on the right for the **Application
   Security >> Proactive Bot Defense** menu and select **Always**
   from the drop-down menu for **Operation Mode**.

   .. image:: images/image1_3_6.PNG

#. Notice that for **Block requests from suspicious browsers** the
   **Block Suspicious Browsers** setting is enabled by default.

#. Click the **Update** button to complete the Proactive Bot
   Defense ``webgoat_DoS`` profile.

   .. image:: images/image1_3_7.PNG

Apply Proactive Bot Defense Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


#. Under **Local Traffic > Virtual Servers**, click
   on ``webgoat.f5demo.com_https_vs``.

#. Click on **Policies** under the **Security** tab at the top of
   the ``webgoat.f5demo.com_https_vs`` details menu.

#. In the **DoS Protection Profile** drop down menu,
   select ``Enabled...`` and then select the ``webgoat_DoS`` for
   the profile.

#. Click on the **Update** button to apply the policy.

   .. image:: images/image1_3_8.PNG

Create Bot Defense Logging Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


#. Open a new tab for the Configuration Utility and navigate to:
    **Security > Event Logs > Logging Profiles** then **click
   the plus icon**.

#. Enter a Profile Name ``bot-defense_allrequests``, select the
   checkbox for ``Bot Defense``.

#. Under the **Bot Defense** logging section, select the checkboxes
   for the following: ``Local Publisher``, ``Log Illegal Requests``, and
   ``Log Challenged Requests``.

#. Click **Finished**.

   .. NOTE:: You could have also modified the existing ``asm_allrequests``
      logging profile and added DoS logging definitions.

   .. image:: images/image1_3_9.PNG

Apply Bot Defense Logging Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


#. Under **Local Traffic > Virtual Servers**, click
   on ``webgoat.f5demo.com_https_vs``.

#. Click on **Policies** under the **Security** tab at the top

#. Within the Available logging profiles menu,
   select ``bot-defense_allrequests`` and then click
   the ``<<`` arrows to move the logging policy to
   the ``Selected`` profile.

#. Click on the **Update** button to apply the policy.

   .. NOTE:: You can associate multiple logging profiles with a given
      virtual server. F5 allows for an incredible amount of logging
      flexibility. Most commonly you would have DoS, Bot Defense and ASM
      Security Policy events logged to a centralized SIEM platform, but
      there may be additional logging requirements such as a web team that
      would be interested in Bot Defense logs solely, while the SIEM
      continues to receive the union of DoS, Bot Defense and ASM Security
      Policy events.

   .. image:: images/image1_3_10.PNG

Test the Proactive Bot Defense Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


#. From the command line execute the following command several times:

   ``curl –k https://webgoat.f5demo.com/WebGoat/login``

   .. NOTE:: This can take a few minutes and you may get several empty
      responses as shown.

   After a few moments the PBD will initialize and you will Because
   Proactive BOT Defense is always on, this tool will always be
   blocked.


Validate that the Proactive Bot Defense Policy is Working
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


#. Navigate to **Security > Event Logs > Bot Defense > Requests**.


#. Notice that the detected bot activity has been logged and is now
   being displayed for review.

    .. image:: images/image1_3_11.PNG

#. Note the stated reason for the request being blocked. You may have to
   scroll to the right to see this reason. What was the stated reason?


BOT Signatures
~~~~~~~~~~~~~~


#. Navigate to **Security > DoS Protection > DoS Profiles**


#. **Click** on the ``webgoat_DoS`` profile and then the
   **Application Security** tab to configure the policy.

#. Select **Proactive Bot Defense** under the list of **Application
   Security** options.

#. In the **Application Security >> Proactive Bot Defense**
   section, click the **Edit** link for **Operation Mode** and
   then change the setting from **Always** to **During Attack** and
   click **Update** to complete the policy change.

   .. NOTE:: Ignore the DNS Resolver warning

   .. image:: images/image1_3_12.PNG

#. Run cURL again: ``curl –k https://webgoat.f5demo.com/WebGoat/login``

   **The site should respond normally now every time.**

#. cURL is considered an **HTTP Library tool** and falls in **the Benign
   Category**.


.. NOTE:: Just how benign are HTTP library tools? cURL can easily be
   scripted in a variety of ways and can be used as a downloader to siphon
   off data. Remember the famous media defined “hacking tool” that Snowden
   used? wget? There are many use-cases where you simply do not want a tool
   interacting with your site.

Selectively Blocking BOT Categories
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


#. Under your ``webgoat_DoS`` profile in **Application Security >> Bot
   Signatures** click on the **Edit** link for the **Bot Signature
   Categories** section.

   .. image:: images/image1_3_13.PNG

#. Change the HTTP Library action from **None** to **Block** under
   the **Benign Categories** section and click **Update** to apply
   the policy changes.

   .. image:: images/image1_3_14.PNG

#. Run cURL again: ``curl --insecure https://webgoat.f5demo.com/WebGoat/login``

   .. image:: images/image1_3_15.PNG

   Whammo!!!... as soon as the BOT is revealed... the connection is dropped.
   The TLS doesn’t get established.

   Let’s say we actually DO want to allow cURL or another automated
   tool. We may have developers that rely on curl so let’s whitelist
   just that.

**To Whitelist cURL:**

#. Edit the **Bot Signatures** list and find **curl**. Move it
   to disabled signatures and click **Update**.

   .. image:: images/image1_3_16.PNG


#. Run cURL again: ``curl --insecure https://webgoat.f5demo.com/WebGoat/login`` and you should
   be back in business. By now you should know the expected output.

#. Change HTTP Library to: **Report** and remove **CURL** from the whitelist and set http libraries category to ``report``.

.. image:: images/image1_3_17.PNG

#. Modify the ``webgoat_DOS`` Dos Profile operation Operation Mode to: ``Always`` and click **Update**.

.. image:: images/image1_3_18.PNG

cURL from Different Geolocations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  .. NOTE:: We are going to leverage the ``webgoat_irule`` from the earlier lab to get some randomness.

#. Open **Local Traffic > Virtual Servers** and click on ``webgoat.f5demo.com_https_vs``. Go to the **Resources** horizontal tab and click on **Manage** in the **iRules** section.

  .. image:: images/image1_3_19.PNG

#. Select the ``webgoat_irule``, move it to the **Enabled** assignment and click **Finished**.

#. Run the cURL command several times: ``curl --insecure https://webgoat.f5demo.com/WebGoat/login``


#. Review the event logs at **Event Logs >> Bot Defense** You will
   now see geo-data for the BOT connection attempts.

   .. image:: images/image1_3_20.PNG

#. Navigate to **Security > Overview** and review the default
   report elements.

#. Click **Overview > Application > Traffic**:

   .. image:: images/image1_3_21.PNG

#. Take some time reviewing this screen and practice adding a new widget
   to see additional reporting elements:


#. Click the **DoS tab** at the top. The DOS Visibility Screen loads.

   .. image:: images/image1_3_22.PNG

   .. NOTE:: You may need to change your time in the Windows system tray for
      accurate results.

   Although there have not been any L7 DoS attacks some of the widgets
   along the right contain statistics from the BOT mitigations.

#. Click the **Analysis** tab at the top and review the graphs
   available to you.

   .. image:: images/image1_3_23.PNG

#. Click the **URL Latencies** tab at the top and review the graphs
   available to you.

   .. image:: images/image1_3_24.PNG

#. Click the **Custom Page** tab at the top and review the graphs
   available to you.

   Please feel free to add widgets and/or explore the ASM interface
   further.

**This concludes the BOT Protection section of this lab guide!**
