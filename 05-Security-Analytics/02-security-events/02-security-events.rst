Investigate security events
###########################

Use Security Analytics to understand why requests were allowed, flagged, rate limited, or blocked.

1. In **Web App & API Protection**, go to **Overview** > **Security**.
2. In **Delivery Resources**, select the ``arcadia-finance`` HTTP load balancer and open **Security Analytics**.
3. Set the time range to **Last 24 hours**, select **Apply**, and refresh the dashboard.
4. Locate and expand representative events from each completed security exercise:

   * A WAF signature match.
   * An API Protection validation or endpoint-policy violation.
   * An API rate-limit event.
   * A Bot Defense classification or mitigation event.

5. For each event, review the request details, detected violation or classification, matching policy or rule, enforcement action, response code, and request ID.
6. Use dashboard filters to compare allowed and denied traffic and to distinguish WAF, API Protection, rate-limit, and Bot Defense events.
7. Use **Forensics** to narrow the results to a path, source, action, or other relevant event attribute and investigate related traffic.

Security events can take a short time to appear. If an expected event is absent, confirm the relevant policy is attached and enforced, repeat only the corresponding lab test, and refresh the dashboard.
