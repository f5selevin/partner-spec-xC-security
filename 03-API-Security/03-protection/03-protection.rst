Enforce the Arcadia Finance Open Banking API OpenAPI schema
###########################################################


Configure tha API Protection
=============================
1. Before enabling API Protection, establish that the origin currently handles both documented and undocumented paths:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      curl -i "$BASE/banks"
      echo 
      curl -i "$BASE/banks/unknown-test/extra"
      echo 
      curl -i -X DELETE "$BASE/banks"

   Record both baseline status codes and response bodies. The undocumented path will return an error ``400``; after protection is enabled, F5 XC must reject it before it reaches the origin. The protected response can also use status ``404``, so use the F5 XC block page and support ID—not the status code alone—to distinguish it from the origin response.



2. Open Edit configoration for the arcadia finance HTTP load balancer.

3. Click **Api Protection (1)**. Select **Enable** for **API Definition (2)**, choose just 
   created **API Definition: (3)** ``$$namespace$$/arcadia-finance-api``, and select **API Inventory** 
   for **Validation (4)**. Click on **View Configuration > (5)** to set the request fallback to block.
.. image:: ../../img/api-protection-details-1.png   
   :align: center 

4. First, select **Block** for **Request Validation Enforcement Type (1)**. Then add **HTTP Body** to 
   **Request Validation Properties (2)**. In **Fallback Through Mode (3)** select **Custom**. 
   Click on **Configure > (4)** to set custom rules.

.. image:: ../../img/api-protection-details-2.png   
   :align: center

5. In the appeared dialog click **Add Item** and then fill in the form. Specify ``arcadia-finance-api-fallback`` for 
   the **Name (1)**. Select **Block** for **Action (2)**, **Base Path (3)** for **Type**. Specify ``/`` 
   as a **Base Path (4)**. Click **Apply (5)** to save the rule.

.. image:: ../../img/api-protection-details-3.png   
   :align: center

6. The custom rule will be added. Click **Apply (1)** to save the configuration.
.. image:: ../../img/api-protection-details-4.png   
   :align: center


7. Click **Apply (1)** to save the API Inventory validation configuration.
.. image:: ../../img/api-protection-details-5.png   
   :align: center

8. Click **Save HTTP Load Balancer (1)** to save the updates.
.. image:: ../../img/api-protection-details-6.png   
   :align: center


Test the API Protection
=========================
1. Confirm a documented endpoint remains available:

   .. code-block:: console
      curl -i "$BASE/banks"

2. Confirm an unknown endpoint is rejected by F5 XC:

   .. code-block:: console
      curl -i "$BASE/banks/unknown-test/extra"

3. Send a method not defined for a known endpoint and verify enforcement:

   .. code-block:: console
      curl -i -X DELETE "$BASE/banks"

4.  Send a valid payment body and confirm it passes API validation:

   .. code-block:: console
      curl -i -X POST "$BASE/payments" \
        -H 'Content-Type: application/json' \
        -d '{"amount":100.50,"currency":"EUR","recipient":{"name":"Lab Recipient","accountNumber":"12345678","bankCode":"LABBANK1"}}'

5. Send a body with schema type violations and confirm it is rejected:

   .. code-block:: console
      curl -i -X POST "$BASE/payments" \
        -H 'Content-Type: application/json' \
        -d '{"amount":"not-a-number","currency":123,"recipient":false}'