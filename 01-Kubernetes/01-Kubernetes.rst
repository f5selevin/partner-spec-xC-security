Deploy the Arcadia Finance Open Banking API on Virtual Kubernetes
#################################################################

In this section you create an F5 XC Virtual Kubernetes environment, deploy the Arcadia Finance Open Banking API as three services plus a Swagger UI service, and publish them through one HTTP load balancer. The public GitHub Container Registry images provide separate Banks, Accounts, Payments, and interactive API documentation workloads. This establishes the application used by every security lab that follows.

.. toctree::
   :maxdepth: 1
   :glob:

   [0-9][0-9]-*/[0-9][0-9]-*
