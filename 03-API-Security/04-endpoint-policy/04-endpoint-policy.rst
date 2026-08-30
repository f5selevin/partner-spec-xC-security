Deny a sensitive API endpoint
#############################

This exercise adapts the explicit endpoint-denial test from the source API Protection lab. Assume that a review has found ``/user/profile`` exposed before its banking-data handling has been approved.

1. Before adding the rule, establish the baseline:

   .. code-block:: console
      BASE="https://$$namespace$$.spec-security.f5se.com"
      curl -i "$BASE/user/profile"

   Confirm that the documented endpoint reaches the application.

2. Manage the ``arcadia-finance`` HTTP load balancer and edit its configuration.
3. Under **API Protection** > **API Protection Rules**, configure **API Endpoints** and add:

   .. table:: Required endpoint policy settings
      :widths: auto

      ==================  =========================
      Setting             Value
      ==================  =========================
      Name                ``deny-user-profile``
      Action              Deny
      API endpoint        ``/user/profile``
      Method list         ANY
      ==================  =========================

4. Apply each configuration panel and save the HTTP load balancer.
5. Repeat the baseline request several times to produce events:

   .. code-block:: console
      curl -i "$BASE/user/profile"

6. Confirm that the request is denied, normally with HTTP ``403``, while another documented endpoint still works:

   .. code-block:: console
      curl -i "$BASE/banks"

This rule demonstrates an emergency control for an approved endpoint whose implementation or data handling must be remediated. It is independent of API Discovery.
