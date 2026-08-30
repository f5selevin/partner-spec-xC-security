Test WAF and inspect the event
##############################

1. Confirm a normal request still succeeds:

   .. code-block:: console
      curl -i "https://$$namespace$$.spec-security.f5se.com/banks"

2. Send a test SQL-injection string in an account lookup query:

   .. code-block:: console
      curl -i --get \
        --data-urlencode "search=' OR 1=1--" \
        "https://$$namespace$$.spec-security.f5se.com/accounts"

3. Confirm that F5 XC returns a blocking response, normally HTTP ``403``. Do not send attack tests to systems outside this lab.
4. In **Web App & API Protection** > **Overview** > **Security**, select the ``arcadia-finance`` load balancer and adjust the time range if necessary.
5. Filter by the request path ``/accounts``. Open the event and identify the source, HTTP method, matched attack type or signature, enforcement action, and request ID.
6. If the request was allowed, verify that the policy is attached, enforcement is blocking, and the matching signature is not staged; then repeat the test.
