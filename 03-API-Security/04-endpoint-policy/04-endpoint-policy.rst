Deny a sensitive API endpoint
#############################

During a pre-release review of the Account API, the team discovers that the
``/api/accounts/{accountId}/balance`` route is exposed. Because this route returns sensitive
banking data, it must remain unavailable until the required approval is granted.

Create a deny API endpoint rule
===============================

1. Before adding the rule, establish a baseline:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      curl -i "$BASE/api/accounts/1001/balance"

2. Open the configuration editor for the ``arcadia-finance`` HTTP load balancer.

3. Under **API Protection (1)**, click **Configure (2)** for **API Protection Rules**.

   .. image:: ../../img/api-protection-rules-details-1.png
      :align: center

4. Click **Configure (1)** to add an **API Endpoints** rule.

   .. image:: ../../img/api-protection-rules-details-2.png
      :align: center

5. Enter a **Name (1)** and select **Deny (2)** for **Action**. Enter
   ``/api/accounts/{accountId}/balance`` for **API Endpoint (3)**, select **ANY (4)**
   for **Method List**, and then click **Apply (5)** to save the rule.

   .. table:: Required endpoint policy settings
      :widths: auto

      ==================  =========================
      Setting             Value
      ==================  =========================
      Name (1)            ``deny-account-balance``
      Action (2)           Deny
      API Endpoint (3)    ``/api/accounts/{accountId}/balance``
      Method List (4)      ANY
      ==================  =========================

   .. image:: ../../img/api-protection-rules-details-3.png
      :align: center

6. Click **Apply (1)** to save the API endpoint configuration.

   .. image:: ../../img/api-protection-rules-details-4.png
      :align: center

7. Click **Save HTTP Load Balancer (1)** to save the changes.

   .. image:: ../../img/api-protection-rules-details-5.png
      :align: center

Test the deny API endpoint rule
===============================
1. Repeat the baseline request several times to generate events:

   .. code-block:: console
      curl -i "$BASE/api/accounts/1001/balance"

2. Confirm that the request is denied, typically with an HTTP ``403`` response, while
   another documented endpoint remains accessible:

   .. code-block:: console
      curl -i "$BASE/api/banks"
