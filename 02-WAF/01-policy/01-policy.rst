Create and attach an application firewall
#########################################

1. In **Web App & API Protection**, go to **App Firewall** and select **Add App Firewall**.
2. Name the policy ``arcadia-openbanking-waf``.
3. Use the default signature set, **High** and **Medium** threat levels, and blocking enforcement. Enable attack-signature staging only if directed by the instructor; staged signatures do not block the validation request.
4. Save the firewall.
5. Go to **Load Balancers** > **HTTP Load Balancers**, manage ``arcadia-openbanking``, and select **Edit Configuration**.
6. Under **Web Application Firewall**, select **Enable** and attach ``arcadia-openbanking-waf``.
7. Save the HTTP load balancer and allow time for the configuration to propagate.

The firewall now inspects both application and API traffic before requests reach the vK8s workload.
