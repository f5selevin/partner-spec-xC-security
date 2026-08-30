Deploy the Arcadia Finance Open Banking API on Virtual Kubernetes
#################################################################

In this section you create an F5 Distributed Cloud Services Virtual Kubernetes (vK8s) environment, deploy the Arcadia Finance Open Banking API as three services plus a Swagger UI service, and publish them through one HTTP load balancer. The public GitHub Container Registry images provide separate Banks, Accounts, Payments, and interactive API documentation workloads. This establishes the application used by every security lab that follows.

F5 Distributed Cloud Customer Edge (CE) sites that can run Kubernetes workloads are available as **Secure Mesh Site v2 (SMSv2)** and **App Stack Site** deployments. This lab uses an SMSv2 site. To place vK8s workloads on a CE site, you must first create a **Virtual Site**. The Virtual Site selects and maps to the target CE site through labels. In this lab, the site selector uses the ``ves.io/siteName`` label to map the Virtual Site to the SMSv2 site.

Exercise Steps

.. toctree::
   :maxdepth: 1
   :glob:

   [0-9][0-9]-*/[0-9][0-9]-*
