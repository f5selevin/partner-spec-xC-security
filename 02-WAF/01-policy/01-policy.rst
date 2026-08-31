Create and attach an App Firewall
#################################

Create the App Firewall
=======================

Create an App Firewall with F5 Distributed Cloud Web App and API Protection
(WAAP). The firewall inspects application traffic and blocks requests that
violate the configured security policy.

1. In the F5 Distributed Cloud Console, open the **Web App & API Protection**
   workspace. Select **Manage** > **App Firewall (1)**, and then select **Add App
   Firewall (2)**.

   .. image:: ../../img/waap-add.png
      :align: center
      :alt: App Firewall page in the Web App and API Protection workspace with the Add App Firewall button.

2. Enter ``arcadia-finance-waf`` in the **Name (1)** field. Set **Enforcement
   Mode** to **Blocking (2)**. By default, WAF returns HTTP status code 200 and
   displays the block page content when blocking a request. To return HTTP
   status code 403 instead, select **Custom (3)** under **Blocking Response
   Page**, and set **Response Code (4)** to **403 Forbidden**. Click **Add App
   Firewall (5)**.

   .. image:: ../../img/waap-add-details-1.png
      :align: center
      :alt: App Firewall form configured with the arcadia-finance-waf name, Blocking enforcement mode, and Default security policy.

3. ``arcadia-finance-waf`` will appear on the **App Firewall** page. Click **Load Balancers (1)**, and then select **HTTP Load Balancers (2)**.

   .. image:: ../../img/waap-add-result.png
      :align: center
      :alt: App Firewall page showing the newly created arcadia-finance-waf firewall.

Attach the App Firewall to the HTTP Load Balancer
=================================================

Associate the App Firewall with the Arcadia Finance HTTP Load Balancer so that
F5 Distributed Cloud WAAP inspects traffic before it reaches the application.

1. Click **... (1)** next to the Arcadia Finance HTTP Load Balancer, and then select **Manage Configuration (2)**.

   .. image:: ../../img/waf-lb-manage-config.png
      :align: center
      :alt: HTTP Load Balancers page with Manage Configuration selected for the arcadia-finance load balancer.

2. On the HTTP Load Balancer configuration page, select **Edit Configuration (1)**.

   .. image:: ../../img/waf-lb-edit-config.png
      :align: center
      :alt: Configuration view for the arcadia-finance HTTP load balancer with the Edit Configuration action.

3. Click **Web Application Firewall (1)** and enable the feature **(2)**. Select
   ``$$namespace$$/arcadia-finance-waf`` from the list **(3)**. Click **Save HTTP Load
   Balancer (3)** to update the load balancer settings.

   .. image:: ../../img/waf-lb-enable-waf.png
      :align: center
      :alt: Web Application Firewall settings enabled with arcadia-finance-waf selected for the HTTP load balancer.

.. note::
   Later tasks return to the HTTP Load Balancer configuration. When instructed
   to **open the HTTP Load Balancer configuration**, repeat steps 1 and 2 in
   this section.