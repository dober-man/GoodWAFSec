Lab 1.1 Base Policy Creation
----------------------------
Objective
---------

- Create a transparent rapid deployment policy.

- Enable applicaiton security logging profile.

- Validate that both the policy and logging profile are working.

- Review the auto-detection of the web server capabilities (i.e. Apache, jQuery).

- Estimated time for completion: **30** **minutes**.

This lab will demonstrate how to create and build a transparent security policy.
Please ensure that three virtual servers are configured before you begin:

- ``webgoat.f5demo.com_https_vs``
- ``webgoat.f5demo.com_http_vs``
- ``ip_rep_target_https_vs``

1.1.1 Create Policy
~~~~~~~~~~~~~~~~~~~
. IMPORTANT:: To clearly demonstrate just the Bot Defense profile,
   please **disable** the Application Security Policy from the
   ``webgoat.f5demo.com_https_vs`` virtual server!

#. On the Main tab, click **Security > Application Security > Security Policies**. The Active Policies screen opens.
#. Click on the **Polices List**
   |image43|

#. Click on the **Create New Policy** button. The policy creation wizard opens.

   |image44|

#. Click on the **Advanced** button (Top-Right) to ensure that all the available policy creation options are displayed.
#. Name the security policy ``lab1_webgoat_waf`` and ensure that the **Policy Type** is ``security``.
#. Verify the **Policy Template** is set to ``Rapid Deployment Policy``.
#. Assign this policy to the webgoat.f5demo.com_https_vs from the Virtual Server drop down.
#. Set the Application Language to **UTF-8**.
#. Go back two settings
#. Set the **Enforcement Mode** to ``Transparent``.
#. Accept the remaining default policy settings.

**Your settings should reflect the figure below:**

  |imagexx|

#. Click **Create Policy** to complete the policy creation process.
#. After policy creation is complete, the properties will be displayed for review within the Policies List menu.
#. Click **Apply** while the ``lab1_webgoat_waf`` policy is selected.

1.1.2 Verify WAF Profile is Applied to Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. In the configuration utility navigate to **Local Traffic> Virtual Servers**, click on ``webgoat.f5demo.com_https_vs``.

#. Click on **Polices** under the **Security** tab at the top of the ``webgoat.f5demo.com_https_vs`` details menu.

#. In the **Application Security Policy** drop down menu, ensure **Application Security Policy** is ``Enabled...`` and the **Policy:** drop-down selection shows the ``webgoat.f5demo.com_https_vs`` policy.

#. Notice Log Profile is set to ``Disabled``.

1.1.3 Create Application Security Logging Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. In the configuration utility navigate to **Security > Event Logs > Logging Profiles** then click on the **plus** icon.

#. Under the **Logging Profile Properties** section enter a **Profile Name** ``waf_allrequests``, select the checkbox for ``Application Security``.

#. Change the **Configuration** dropdown to ``Advanced`` under the **Application Security** section.

#. Select the ``Local Storage`` value for the **Storage Destination** configuration option.

#. Select the ``For all Requests`` value for the **Response Logging** configuration option.

#. Select the ``All requests`` value for the **Request Type** configuration option.

#. Click **Finished.**

  |imagexy|

**Question:** Would logging all requests and responses in a production environment be a best practice?

1.1.4 Apply WAF Logging Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. Under Local Traffic > Virtual Servers, click on hackazon.f5demo.com_https_vs.
#. Click on Policies under the Security tab at the top of the hackazon.f5demo.com_https_vs details menu.
#. In the Log Profile drop down menu, select Enabled...
#. Within the Available logging profiles menu, select asm_allrequests and then click the << arrows to move the logging policy to the Selected profile.
#. Click on the Update button to apply the policy.

1.1.5 Test WAF Policy
~~~~~~~~~~~~~~~~~~~~~


1.2 Geolocation and IP Intelligence
----------------------------------------
1.2.1 Geolocation
~~~~~~~~~~~~~~~~~~

#. Open **Security > Application Security > Geolocation Enforcement**

#. Select all geolocations **except the United States and N/A** and move
   them to Disallowed Geolocations. **Save** and then **Apply Policy**.

   .. NOTE:: N/A covers all RFC1918 addresses. If you aren’t dropping them
      at your border router (layer 3), you may decide to geo-enforce at
      ASM (Layer 7) if no private IP’s will be accessing the site.

   |image34|

#. Open **Local Traffic > iRules** and open the iRule titled
   ``webgoat_irule`` and review the code.

   .. code-block:: tcl
      :linenos:

      when HTTP_REQUEST {
         HTTP::header replace X-Forwarded-For "[expr (int(rand()*221)+1)].[expr int(rand()*254)].[expr int(rand()*254)].[expr int(rand()*254)]"
      }

   .. NOTE:: The above iRule is essentially scanning the HTTP headers and when
      it finds the ``X-Forwarded-For`` header it will replace the original source
      IP address with a randomized IP address. Since we are only manipulating
      the header this has no discernable affect on traffic flow. This iRule
      event, ``when HTTP_REQUEST``, also fires before the ASM policy allowing
      this "trick" to work to demonstrate a global range of source IP
      addresses.

   |image35|

#. Open **Local Traffic > Virtual Servers** and click on
   ``webgoat.f5demo.com_https_vs``. Go to the **Resources**
   horizontal tab and click on **Manage** in the iRules section.

   |image36|

#. Select the ``webgoat_irule``, move it to the **Enabled** assignment and
   click **Finished**.

   |image37|

#. In a **new Firefox Private Browsing window** connect to
   ``https://webgoat.f5demo.com``. You may need to connect more than
   once to receive the block page, make a note of the last four digits
   of the Support ID. Why did you receive the block page?

#. In the BIG-IP Administrative Interface go to **Security > Event Logs
   > Application > Requests** and click on the magnifying glass to
   expand the search filter. Enter the Support ID and click **Apply Filter**.

   |image38|

   Notice the geolocation detected and the presence of the X-Forwarded-For
   (XFF) in the Request details. Your actual client IP is still
   10.128.10.100 however, because we trusted the XFF header and the iRule
   is randomizing the IP address placed in that header.

   ASM believes the request is from an external location to provide a more
   realistic example. Depending on your network you may be leveraging a
   technology that creates a source NAT ahead of ASM so by leveraging the
   XFF you can work around this and get contextual information about the
   client.

   |image39|

.. IMPORTANT:: Please remove the iRule ``webgoat_irule`` from the
   Virtual Server before proceeding to the next step. (Virtual Server >
   Resources)

   IP Reputation
   -------------

   #. Navigate to **Security > Application Security > IP Addresses > IP
      Address Intelligence** and click **Enabled**. For all categories
      **select Alarm**. Click on **Save** and then on **Apply Policy**.

      .. NOTE:: On the top right you should see that your IP Intelligence
         database has been updated at some point.

      |image40|

      .. NOTE:: In order to create traffic with malicious sources for the purposes of
         this lab we have created added additional configuration items for you.

      There is a Virtual Server (VS) called ``ip_rep_target_https_vs`` which
      has a SNAT pool predefined with 5 known malicious IP addresses.

      There is an iRule applied to that VS which then points the traffic to
      the VS you have been working on ``webgoat.f5demo.com_https_vs`` which has
      your ASM policy applied. This configuration will cause ASM to see the
      inbound traffic as having the malicious sources.

   #. Please review the Virtual Server configuration for
      ``ip_rep_target_https_vs``. No changes are needed. Also, please
      review the iRule assigned under the VS Resource tab.

   #. Open a new private browsing window in Firefox and use the bookmark
      for **IP Rep Lab** to browse the site. Click on one or two items
      until you get the block page.

      |image41|

   #. Navigate to **Security > Event Logs > Application > Requests** and
      review the log entries. Since you configured IP Intelligence
      violations to alarm you will not need change the filter. Select the
      most recent entry and examine why the request is illegal. What IP
      address did the request come from?

      |image42|

      **Bonus:** You can browse to ``http://www.brightcloud.com/tools/url-ip-lookup.php``
      and look up the IP address in question for further information. There is also
      a tool to report IP addresses that have been incorrectly flagged.

      Further, you can use Putty on the Win7 box to access the BIG-IP via SSH
      (bookmarked as F5-WAF) and login with ``root`` / ``401elliottW!`` to run
      the ``iprep_lookup`` command, similar to:

      .. code-block:: console

         [root@bigip1:Active:Standalone] config # iprep_lookup 77.222.40.121
         opening database in /var/IpRep/F5IpRep.dat
         size of IP reputation database = 39492859
         iprep threats list for ip = 77.222.40.121 is:
         bit 7 - Phishing
         bit 8 - Proxy
