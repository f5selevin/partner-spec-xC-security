Compare automated and browser traffic
#####################################

1. Send repeated scripted POST requests to the protected payment endpoint. Use only the lab application:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      for i in $(seq 1 20); do
        curl -sS -o /dev/null -X POST \
          -H 'Content-Type: application/json' \
          -d '{"amount":100,"currency":"EUR","reference":"lab-test"}' \
          "$BASE/payments"
      done

2. Open the Swagger UI at :ext_link:`https://$$namespace$$.spec-security.f5se.com/swagger/` in a JavaScript-capable browser and use **Try it out** to submit a payment request. Do not use real banking data.
3. In **Web App & API Protection** > **Overview** > **Security**, select the load balancer and filter for method ``POST`` and path prefix ``/payments``.
4. Open **Bot Defense** details and compare automation classification, client type, telemetry availability, and mitigation action for the scripted and browser-generated requests. Direct tools such as ``curl`` cannot execute the Bot Defense JavaScript telemetry challenge.
5. After confirming expected classification, edit the protected endpoint and change mitigation from **Flag** to the instructor-approved blocking action. Save and rerun the scripted test.
6. Confirm the automated request is mitigated, then inspect the event details and request ID in security analytics.

A classification is risk-engine output, not proof of malicious intent. Tune endpoint scope and mitigation based on observed legitimate clients before applying this pattern to production payment APIs.
