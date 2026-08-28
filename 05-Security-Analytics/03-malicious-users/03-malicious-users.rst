Review Malicious User Detection
###############################

Review whether repeated abusive behavior from the preceding exercises contributed to a malicious-user risk score.

1. In **Web App & API Protection**, go to **Overview** > **Security**.
2. In **Delivery Resources**, select the ``arcadia-finance`` HTTP load balancer.
3. Open the **Malicious Users** tab.
4. Set the time range to **Last 24 hours**, select **Apply** if prompted, and refresh the dashboard.
5. Review the listed clients and open any available malicious-user record. Compare its risk score, observed security events, source information, and current mitigation action with the requests generated during this workshop.
6. Where available, pivot from a user record to its related requests or security events and confirm that the details and request IDs are consistent.

.. note::

   A malicious-user score is a risk signal based on observed behavior, not proof of identity or intent. Events may not appear if Malicious User Detection is not enabled for the tenant or if the lab traffic does not meet its scoring thresholds.

Mitigation is not enabled or tuned in this exercise. In a production design, higher scores can trigger actions such as a temporary block, but thresholds and actions should be validated against legitimate client behavior before enforcement.

**End of lab**
