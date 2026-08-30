Apply and test API rate limiting
################################

This exercise adapts the pre- and post-mitigation rate-limit test from the source API Security labs. Protect the account-list operation from a looping or excessively active client.

Baseline test
~~~~~~~~~~~~~

1. Send more than ten requests within one minute:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      for i in $(seq 1 12); do
        code=$(curl -sS -o /dev/null -w '%{http_code}' "$BASE/accounts")
        printf 'request=%02d status=%s\n' "$i" "$code"
      done

2. Before rate limiting is enabled, confirm that the requests receive the normal application status.

Configure the control
~~~~~~~~~~~~~~~~~~~~~

1. Manage the ``arcadia-finance`` HTTP load balancer and edit its configuration.
2. Under **Common Security Controls** > **Rate Limiting**, select **API Rate Limit** and view its configuration.
3. Under **API Endpoints**, add this rule:

   .. table:: Required API rate-limit settings
      :widths: auto

      ==================  =====================
      Setting             Value
      ==================  =====================
      API endpoint        ``/accounts``
      Method list         GET
      Threshold           ``10``
      Duration            Minute
      ==================  =====================

4. Apply each panel and save the HTTP load balancer.

Mitigation test
~~~~~~~~~~~~~~~

1. Wait for any baseline rate window to expire, then rerun the 12-request loop.
2. Verify that requests through the configured threshold are allowed and subsequent requests are rate limited, normally with HTTP ``429``.
3. Confirm that ``/banks`` remains available, demonstrating that the control is scoped to the selected endpoint and method.
4. Repeat the rejected request a few times so the analytics exercise has sufficient events.
