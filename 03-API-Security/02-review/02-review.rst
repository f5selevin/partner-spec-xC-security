Review the approved Arcadia Finance Open Banking API surface
############################################################

1. Open the ``arcadia-finance-api`` API definition.
2. Review the imported operations and confirm that parameterized resources are represented, including ``/accounts/{accountId}``, ``/payments/{paymentId}``, and their transaction, statement, refund, balance, and history subresources.
3. Review the allowed HTTP methods for each path. Pay particular attention to operations that create payments or refunds and operations that expose account or user information.
4. Review path, query, header, and body parameters defined by the schema, including required fields and data types.
5. Confirm that internal or administrative paths are not present in the approved definition.
6. Record the expected behavior for the next exercise:

   * A documented path with an allowed method and valid request shape should reach the Arcadia Finance Open Banking API.
   * An undocumented path should be rejected by the API protection fall-through rule.
   * A documented path using an undefined method should be rejected.
   * A request that violates the schema should be rejected when schema validation is enabled.

This review uses the approved OpenAPI contract only. It does not use learned traffic, a discovered inventory, or sensitive-data discovery.
   