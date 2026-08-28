Create a Bot Defense policy
###########################

Automated payment submission is a realistic abuse case for the Arcadia Finance Open Banking API. Begin in flag mode so you can inspect classification without disrupting the exercise.

1. Manage the ``arcadia-openbanking`` HTTP load balancer and edit its configuration.
2. Under **Bot Protection**, select **Enable Bot Defense Standard** and configure **Protected App Endpoints**.
3. Add this endpoint:

   .. table:: Required Bot Defense endpoint settings
      :widths: auto

      ============================  ==========================
      Setting                       Value
      ============================  ==========================
      Name                          ``openbanking-payments``
      HTTP methods                  POST
      Protocol                      Both
      Path match                    Prefix
      Prefix                        ``/payments``
      Mitigation action             Flag
      Include mitigation headers    Append headers
      ============================  ==========================

4. Retain the default inference and automation-type header names, apply each panel, and save the load balancer.
5. If Bot Defense Standard is not licensed in the workshop tenant, record that limitation and review the configuration with the instructor rather than substituting an unrelated feature.

Flag mode records Bot Defense telemetry and adds inference headers but does not block the request. You will change the action only after validating classification.
