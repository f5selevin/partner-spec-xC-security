Test WAF and inspect the event
##############################

Issue a test SQL injection attack
=================================

1. Open the web shell for the UDF **Jump Host** component and confirm that the
   Arcadia Finance Open Banking API is reachable:
   
   .. code-block:: console
      curl -i "https://$$namespace$$.spec-security.f5se.com/api/banks"

2. Send a test SQL injection string in an account lookup query:

   .. code-block:: console
      curl -i --get \
        --data-urlencode "search=' OR 1=1--" \
        "https://$$namespace$$.spec-security.f5se.com/api/accounts"

3. Confirm that F5 Distributed Cloud Web App and API Protection (F5 XC WAAP)
   blocks the request and returns HTTP status code ``403``.
   
   .. warning::

      The test SQL injection string is a known attack pattern. Do not send it to
      any system outside this lab.

4. Locate **Your support ID is:** in the blocking response and copy the support
   ID. You will use it to find the corresponding security event.

Analyze the attack
==================

Use F5 XC WAAP security analytics to locate the blocked request and review the
WAF detection details.

1. In the **Web App & API Protection** workspace,
   click **Security (1)**. Then scroll down and in the **Delivery Resources** section click the **arcadia-finance**.

   .. image:: ../../img/waap-security.png
      :align: center


2. Select **Security Analytics (1)** to view security events detected for the application.

   .. image:: ../../img/waap-security-analytics.png
      :align: center


3. Click **Add Flter (1)** to enable filtering of the security events.

   .. image:: ../../img/waap-security-analytics-details-1.png
      :align: center


4. In the filters list select **Filter by request ID.**, choose **In** as operator and enter the support ID copied from the blocking response. Click **Apply** to filter the events.

   .. image:: ../../img/waap-security-analytics-details-2.png
      :align: center


5. The event by the support id will be shown. Click the event date link **(1)** and the **Event Details** page will be shown. To see the request details, click **Request (2)** accordion.

   .. image:: ../../img/waap-security-analytics-details-3.png
      :align: center
