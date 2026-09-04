Protect the Arcadia Finance Open Banking API
############################################

Web application and API protection
**********************************

Web application protection detects attacks in HTTP traffic, including SQL
injection and cross-site scripting. It does not define the valid paths, methods,
parameters, or payloads of an API.

API protection validates requests against an API contract. A request can be
valid HTTP and contain no attack signature but still violate that contract by
using an undocumented path, an unsupported method, or an invalid request body.

F5 Distributed Cloud (XC) Web App and API Protection (WAAP) provides both
controls on the HTTP load balancer. Web Application Firewall policies detect
application attacks. API Security uses an OpenAPI definition to enforce the
published API surface.

OpenAPI enforcement
*******************

F5 Distributed Cloud uses the OpenAPI definition as a positive security model:

* Reject requests to undocumented paths or unsupported HTTP methods.
* Validate request parameters and bodies against the defined schema.
* Deny selected operations with endpoint policy.
* Limit request rates by API endpoint.
* Record allowed, blocked, and rate-limited requests in security analytics.

**Exercise steps**


.. toctree::
   :maxdepth: 1
   :glob:

   [0-9][0-9]-*/[0-9][0-9]-*
