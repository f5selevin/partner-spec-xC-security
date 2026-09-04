Create a Bot Defense policy
###########################

Use Bot Defense to block automated payment requests to the Arcadia Finance Open Banking API.

1. Check the response from the unprotected API endpoint:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      API="$BASE/api/payments"

      curl -i "$API"

2. Send 20 POST requests to the lab API. The command displays the status code for each request:

   .. code-block:: console
      for i in $(seq 1 20); do
        code=$(curl -sS -o /dev/null -w '%{http_code}' -X POST \
          -H 'Content-Type: application/json' \
          -d '{"amount":100.50,"currency":"EUR","recipient":{"name":"Lab Recipient","accountNumber":"12345678","bankCode":"LABBANK1"}}' \
          "$API")
        printf 'request=%02d status=%s\n' "$i" "$code"
      done

   Record the status codes for comparison after Bot Defense is enabled.

3. Open the **Edit Configuration** dialog for the ``arcadia-finance`` HTTP load balancer.

4. Select **Bot Protection (1)**. Under **Bot Defense**, select **Enable Bot Defense Standard (1)**, then click **Configure >**.

   .. image:: ../../img/bot-protect-details-1.png
      :align: center

5. Click **Configure > (1)** under **Protected App Endpoints**.

   .. image:: ../../img/bot-protect-details-2.png
      :align: center

6. Click **Add Item** and enter these settings:

   .. table:: Required Bot Defense endpoint settings
      :widths: auto

      ============================  ==========================
      Setting                       Value
      ============================  ==========================
      Name (1)                      ``finance-payments``
      HTTP methods (2)              POST
      Endpoint Label (3)            Undefined
      Protocol (4)                  Both      
      Prefix (5)                    ``/api/payments``
      Mitigation action (6)         Block
      Status (7)                    403 Forbidden
      ============================  ==========================

   .. image:: ../../img/bot-protect-details-3.png
      :align: center

   Click **Apply**.

7. When the **App Endpoint Type** appears in the list, click **Apply (1)**.

   .. image:: ../../img/bot-protect-details-4.png
      :align: center

8. Click **Apply (1)** once more.

   .. image:: ../../img/bot-protect-details-5.png
      :align: center

9. Click **Save HTTP Load Balancer (1)**.

   .. image:: ../../img/bot-protect-details-6.png
      :align: center