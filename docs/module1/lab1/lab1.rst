Exercise 1.1: Policy Creation
----------------------------------
Objective
~~~~~~~~~

- Create a transparent rapid deployment policy.

- Enable application security logging profile.

- Validate that both the policy and logging profile are working.

- Review the auto-detection of the web server capabilities (i.e. Apache, IIS, MySQL etc).

- Estimated time for completion: **30** **minutes**.

#. From the jumpbox, launch Chrome, click the BIG-IP bookmark and login to TMUI. admin/f5DEMOs4u!

Please ensure that three virtual servers are configured before you begin:

- ``webgoat.f5demo.com_https_vs``
- ``webgoat.f5demo.com_http_vs``
- ``automation_vs``

Create Policy
~~~~~~~~~~~~~

#. On the Main tab, click **Security > Application Security > Security Policies**. The Active Policies screen opens.
#. Click on the **Polices List**

.. image:: images/image1.PNG


#. Click on the **Create New Policy** button. The policy creation wizard opens.

#. Click on the **Advanced** button (Top-Right) to ensure that all the available policy creation options are displayed.

#. Name the security policy ``lab1_webgoat_waf`` and notice that the **Policy Type** is security.

#. Verify the **Policy Template** is set to ``Rapid Deployment Policy``. and notice it is a transparent security policy by default

#. Assign this policy to the ``webgoat.f5demo.com_https_vs`` from the Virtual Server drop down.

#. Set the Application Language to **UTF-8**.

#. Accept the remaining default policy settings and click **Create Policy** to complete the policy creation process.

.. Note:: After policy creation is complete, the properties will be displayed for review within the Policies List menu.

**Your settings should reflect the figures below:**

.. image:: images/image2.PNG

.. image:: images/imagefix.PNG


Verify WAF Profile is Applied to Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. In the configuration utility navigate to **Local Traffic> Virtual Servers**, click on ``webgoat.f5demo.com_https_vs``.

#. Click on **Polices** under the **Security** tab at the top of the ``webgoat.f5demo.com_https_vs`` details menu.

#. In the **Application Security Policy** drop down menu, ensure **Application Security Policy** is ``Enabled...`` and the **Policy:** drop-down selection shows the ``lab1_webgoat_waf`` policy.

#. Notice Log Profile is set to ``Disabled``.

.. image:: images/image4.PNG

Create Application Security Logging Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. In the configuration utility navigate to **Security > Event Logs > Logging Profiles** then click on the **plus** icon.

#. Under the **Logging Profile Properties** section enter a **Profile Name** ``waf_allrequests``, select the checkbox for ``Application Security``.

#. Change the **Configuration** dropdown to ``Advanced`` under the **Application Security** section.

#. Select the ``Local Storage`` value for the **Storage Destination** configuration option.

#. Select the ``For all Requests`` value for the **Response Logging** configuration option.

#. Select the ``All requests`` value for the **Request Type** configuration option.

#. Click **Finished.**

  .. image:: images/image5.PNG

**Question:** Would logging all requests and responses in a production environment be a best practice?

**Answer:** Not typically. This adds 50% or more to the overhead on the log engine and would not typically be used outside of troubleshooting and high security environemnts that are appropriately sized.


Apply WAF Logging Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. Under **Local Traffic > Virtual Servers**, click on ``webgoat.f5demo.com_https_vs``.
#. Click on **Policies** under the **Security** tab at the top of the ``webgoat.f5demo.com_https_vs`` details menu.
#. In the **Log Profile** drop down menu, select ``Enabled...``
#. Within the **Available** logging profiles menu, select ``waf_allrequests`` and then click the **<<** arrows to move the logging policy to the **Selected** profile.
#. Click on the Update button to apply the policy.

.. image:: images/image6.PNG

Test WAF Policy
~~~~~~~~~~~~~~~~~~~~~
#. Open the Google Chrome browser and navigate to ``https://webgoat.f5demo.com/WebGoat/login`` You'll find a toolbar shortcut for the webgoat link.

.. image:: images/image7.PNG

2. Login using **webgoat/F5DEMOs4u!** credentials and interact with the webgoat application by browsing. Please refrain from experimenting with the site using any familiar "exploit" techniques.

#. On the BIG-IP, navigate to **Security > Event Logs > Applications > Requests**.

#. Clear the **"Illegal Requests"** filter.
  .. image:: images/image8.PNG

#. Verify that requests are being logged by the WAF. You should be able to see both the raw client and server responses.
  .. image:: images/image9.PNG

Exercise 1.2: Geolocation and IP Intelligence
----------------------------------------
Geolocation
~~~~~~~~~~~

#. Open **Security > Application Security > Geolocation Enforcement**

#. Select all geolocations **except the United States and N/A** and move
   them to Disallowed Geolocations. **Save** and then **Apply Policy**.

   .. NOTE:: N/A covers all RFC1918 addresses. If you aren’t dropping them
      at your border router (layer 3), you may decide to geo-enforce at
      ASM (Layer 7) if no private IP’s will be accessing the site.

   .. image:: images/image10.PNG

   .. IMPORTANT:: Remember to click on the **Apply Policy** button committ security policy changes.

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

#. Open **Local Traffic > Virtual Servers** and click on ``webgoat.f5demo.com_https_vs``. Go to the **Resources**
   horizontal tab and click on **Manage** in the **iRules** section.

   .. image:: images/image11.PNG

#. Select the ``webgoat_irule``, move it to the **Enabled** assignment and
   click **Finished**.

   .. image:: images/image12.PNG

6. We now need to turn on the **Trust XFF Header** feature in the policy.
Navigate to **Application Security > Policy > Policy Properties** and hit the dropdown for **Advanced View**.
You can now check the box to **Trust XFF Header** and click **Save** then **Apply Policy**

.. image:: images/image15.PNG


#. Open a new **Google Chrome Private Browsing** window and connect to
   ``https://webgoat.f5demo.com/WebGoat/login``. Login and select a few links on the WebGoat page.

#. Navigate to **Security > Event Logs > Application > Requests**.

.. image:: images/image13.PNG

   Notice the geolocation detected and the presence of the X-Forwarded-For
   (XFF) in the Request details. Your actual client IP is still
   10.1.10.28 however, because we trusted the XFF header and the iRule
   is randomizing the IP address placed in that header.

   ASM believes the request is from an external location to provide a more
   realistic example. Depending on your network you may be leveraging a
   technology that creates a source NAT ahead of ASM so by leveraging the
   XFF you can work around this and get contextual information about the
   client.

.. IMPORTANT:: Please remove the iRule ``webgoat_irule`` from the
   Virtual Server before proceeding to the next step.

IP Reputation
~~~~~~~~~~~~~

Navigate to **Security > Application Security > IP Addresses > IP Address Intelligence** and click **Enabled**.
For all categories **select Alarm**. Click on **Save** and then on **Apply Policy**.

.. NOTE:: On the top right you should see that your IP Intelligence database has been updated at some point.

.. image:: images/image14.PNG

.. NOTE:: In order to create traffic with malicious sources for the purposes of this lab we have created added additional configuration items for you.

There is an iRule that you will apply to the ``webgoat.f5demo.com_https_vs`` virtual server.
This iRule will insert an X-Forward-For header with value of a malicious source IP address.
This configuration will cause ASM to see the inbound traffic as having the malicious sources.

 #. Navigate to **Local Traffic > Virtual Server > Virtual Servers List** and select the
      ``webgoat.f5demo.com_https_vs`` virtual server.

 #. Navigate to the **Resources** tab and click **Manage** for the **iRules** section.

 #. Move the **ip_rep_irule** irule to the **Enabled** pane of the **Resource Management** configuration.
 Click **Finished**.

 .. image:: images/image16.PNG

 #. Open a new private browsing window in Google Chrome and use the bookmark for **WebGoat** to browse the site.
 Login and Click on one or two items.

 .. image:: images/image17.PNG

 #. Navigate to **Security > Event Logs > Application > Requests** and review the log entries.
 Since you configured IP Intelligence violations to alarm you will not need change the filter.
 Select the most recent entry and examine why the request is illegal. What IP address did the request come from?

 .. image:: images/image18.PNG

**Bonus:** You can browse to ``http://www.brightcloud.com/tools/url-ip-lookup.php``
and look up the IP address in question for further information. There is also
a tool to report IP addresses that have been incorrectly flagged.

Further, you can use Putty on the Win7 box to access the BIG-IP via SSH
(bookmarked as F5-WAF) and login with ``root`` / ``f5DEMOs4u!`` to run
the ``iprep_lookup`` command, similar to:

.. code-block:: rest
{[root@bigip1:Active:Standalone] config #iprep_lookup 77.222.40.121
opening database in /var/IpRep/F5IpRep.dat
size of IP reputation database = 39492859
iprep threats list for ip = 77.222.40.121 is:
bit 7 - Phishing
bit 8 - Proxy'
} 
