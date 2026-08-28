Import the Arcadia Finance Open Banking API definition
######################################################

1. Locate ``arcadia-finance/openbanking/api/openbanking.json`` in the supplied lab files.
2. In **Web App & API Protection**, open **API Management** > **API Definition** and select **Add API Definition**.
3. Name the definition ``arcadia-finance-api``.
4. Add an OpenAPI specification and upload ``openbanking.json`` directly from the local lab files.
5. Confirm that the document parses successfully, then apply and save the API definition.
6. Verify that the definition contains the expected banking resources, including ``/banks``, ``/accounts``, ``/payments``, ``/transactions``, and ``/user/profile``.

.. important::

   Keep API Discovery disabled on the ``arcadia-finance`` load balancer. Do not configure a code-base repository, source-code integration, or GitHub credentials. This lab uses only the directly uploaded OpenAPI document.
