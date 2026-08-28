Enforce the Arcadia Finance Open Banking API OpenAPI schema
###########################################################

1. Before enabling API Protection, establish that the origin currently handles both documented and undocumented paths:

   .. code-block:: console
      BASE="http://$$namespace$$.spec-security.f5se.com"
      curl -i "$BASE/banks"
      curl -i "$BASE/internal/shadow-report"

   Record both baseline status codes. Depending on the mock server, the undocumented path may return an application ``404``; after protection is enabled, it must be rejected at F5 XC rather than passed to the origin.

2. Manage the ``arcadia-finance`` HTTP load balancer and edit its configuration.
3. Confirm that **API Discovery** remains disabled.
4. Enable **API Definition/Protection** and select the previously imported ``arcadia-finance-api`` definition.
5. Configure schema validation and use a deny or block action for requests that do not match the definition. If the console provides a fall-through rule, set it to deny unknown API endpoints.
6. Apply and save the load balancer configuration.
7. Confirm a documented endpoint remains available:

   .. code-block:: console
      curl -i "$BASE/banks"

8. Confirm an unknown endpoint is rejected by F5 XC:

   .. code-block:: console
      curl -i "$BASE/internal/shadow-report"

9. Send a method not defined for a known endpoint and verify enforcement:

   .. code-block:: console
      curl -i -X DELETE "$BASE/banks"

10. Send a valid payment body and confirm it passes API validation:

   .. code-block:: console
      curl -i -X POST "$BASE/payments" \
        -H 'Content-Type: application/json' \
        -d '{"amount":100.50,"currency":"EUR","recipient":{"name":"Lab Recipient","accountNumber":"12345678","bankCode":"LABBANK1"}}'

11. Send a body with schema type violations and confirm it is rejected:

   .. code-block:: console
      curl -i -X POST "$BASE/payments" \
        -H 'Content-Type: application/json' \
        -d '{"amount":"not-a-number","currency":123,"recipient":false}'

12. Repeat each rejected request a few times so it can be located in analytics. The dedicated analytics exercise reviews the resulting events.

.. important::

   The schema is uploaded from the local lab repository. Keep API Discovery disabled, and do not configure **Code Base Integration**, a source repository, or GitHub credentials.
