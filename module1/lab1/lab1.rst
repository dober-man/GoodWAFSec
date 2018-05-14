Lab 1.1 Geolocation and IP Intelligence
----------------------------------------
1.1.1 Geolocation
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
   ``hackazon.f5demo.com_https_vs``. Go to the **Resources**
   horizontal tab and click on **Manage** in the iRules section.

   |image36|

#. Select the ``hackazon_irule``, move it to the **Enabled** assignment and
   click **Finished**.

   |image37|

#. In a **new Firefox Private Browsing window** connect to
   ``https://hackazon.f5demo.com``. You may need to connect more than
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

.. IMPORTANT:: Please remove the iRule ``hackazon_irule`` from the
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
      the VS you have been working on ``hackazon.f5demo.com_https_vs`` which has
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
