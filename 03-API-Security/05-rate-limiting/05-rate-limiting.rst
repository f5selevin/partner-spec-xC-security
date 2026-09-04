Apply and test API rate limiting
################################

In this exercise, you will configure and test rate limiting to protect the payments endpoint from clients that send excessive requests.

Baseline test
~~~~~~~~~~~~~

1. Send more than ten requests in one minute:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      for i in $(seq 1 30); do
        code=$(curl -sS -o /dev/null -w '%{http_code}' "$BASE/api/payments")
        printf 'request=%02d status=%s\n' "$i" "$code"
      done

2. Confirm that, before rate limiting is enabled, all requests receive the application's normal response status.

Configure the control
~~~~~~~~~~~~~~~~~~~~~

1. Open the configuration editor for the ``arcadia-finance`` HTTP load balancer.

2. Under **Common Security Controls (1)**, select **Rate Limiting** and then **API Rate Limit (2)**. Click **View Configuration (3)** to open the configuration panel.

   .. image:: ../../img/api-rate-limit-details-1.png
      :align: center

3. Click **Configure (1)** to add an **API Endpoints** rule.

   .. image:: ../../img/api-rate-limit-details-2.png
      :align: center

4. Click **Add Item** to add a rule. Complete the fields in the dialog using the following values, and then click **Apply (5)** to save the rule.

   .. table:: Required API rate-limit settings
      :widths: auto

      ==================  =====================
      Setting             Value
      ==================  =====================
      API endpoint (1)    ``/api/payments``
      Method list (2)     GET
      Threshold (3)       ``10``
      Duration (4)        Minute
      ==================  =====================

   .. image:: ../../img/api-rate-limit-details-3.png
      :align: center

5. After the rate-limit rule is added, click **Apply (1)** to save the configuration.

   .. image:: ../../img/api-rate-limit-details-4.png
      :align: center

6. Click **Apply (1)** to save the API Endpoints configuration.

   .. image:: ../../img/api-rate-limit-details-5.png
      :align: center

7. Click **Save HTTP Load Balancer (1)** to save the changes.

   .. image:: ../../img/api-rate-limit-details-6.png
      :align: center

Mitigation test
~~~~~~~~~~~~~~~

Run the test again:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      for i in $(seq 1 30); do
        code=$(curl -sS -o /dev/null -w '%{http_code}' "$BASE/api/payments")
        printf 'request=%02d status=%s\n' "$i" "$code"
      done

Confirm that the first ten requests succeed and that subsequent requests are rejected with HTTP status code ``429``.