Publish and verify the Arcadia Finance Open Banking API and Swagger UI
######################################################################

Create a separate origin pool for each Kubernetes Service, then use HTTP routes
to expose the three APIs and Swagger UI through one domain.

1. Click **Select Workspace (1)** and select **Web App & API Protection (2)**.
.. image:: ../../img/workspace-web-app.png
   :align: center


2. In **Web App & API Protection**, go to **Load Balancers** > **Origin Pools**.




2. Create the following four origin pools:

   .. table:: Required origin pools
      :widths: auto

      =================================  =============================================
      Origin pool                        Kubernetes service name
      =================================  =============================================
      ``finance-banks``              ``finance-banks.$$namespace$$``
      ``finance-accounts``           ``finance-accounts.$$namespace$$``
      ``finance-payments``           ``finance-payments.$$namespace$$``
      ``finance-swagger``            ``finance-swagger.$$namespace$$``
      =================================  =============================================

3. Configure each origin pool with port ``80`` and add a **K8s Service Name of
   Origin Server on Given Sites** origin. For each origin, use the Kubernetes
   service name shown in the table, select the site or virtual site associated
   with ``arcadia-finance-vk8s``, and select **vK8s Networks on Site**.
4. Create the HTTP load balancer ``arcadia-finance`` with the domain
   ``$$namespace$$.spec-security.f5se.com``. Configure an HTTP listener on port
   ``80``. Do not enable HTTPS, TLS, automatic certificates, or HTTP-to-HTTPS
   redirection.
5. Add these routes to the HTTP load balancer:

   .. table:: Required HTTP routes
      :widths: auto

      ====================  ============================
      Path prefix           Origin pool
      ====================  ============================
      ``/banks``            ``finance-banks``
      ``/accounts``         ``finance-accounts``
      ``/payments``         ``finance-payments``
      ``/swagger``          ``finance-swagger``
      ====================  ============================

   Prefix matching also sends nested resources, such as
   ``/accounts/{accountId}/balance``, to the correct service.
6. Keep WAF, API Discovery, API Protection, and Bot Defense disabled for this
   baseline.
7. Save the load balancer and wait for provisioning to complete.
8. Verify that the load balancer reaches all four services:

   .. code-block:: console
      export BASE_URL="http://$$namespace$$.spec-security.f5se.com"
      curl -i "${BASE_URL}/banks"
      curl -i "${BASE_URL}/accounts"
      curl -i "${BASE_URL}/payments"
      curl -I "${BASE_URL}/swagger/"

9.  Open the interactive Swagger UI at
   :ext_link:`http://$$namespace$$.spec-security.f5se.com/swagger/`. Expand an
   operation, select **Try it out**, and then select **Execute**. Swagger UI
   sends the request to the matching API route on the same domain.

Successful responses from the API paths and Swagger UI confirm that the load
balancer can discover each in-cluster Service on port ``80``. Each Service
forwards traffic to port ``8080`` on its corresponding container.
