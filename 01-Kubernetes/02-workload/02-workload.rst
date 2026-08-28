Deploy the Arcadia Finance Open Banking API services and Swagger UI from the CLI
#################################################################################

The application consists of three independently built API services and a
Swagger UI service. Each public image is hosted on GitHub Container Registry
(GHCR) and listens on container port ``8080``:

.. table:: Required Arcadia Finance Open Banking API services
   :widths: auto

   ================  =====================  =====================================================================
   Service           Paths                  Public image
   ================  =====================  =====================================================================
   Banks             ``/banks``             ``ghcr.io/f5selevin/arcadia-finance-open-banking/banks:latest``
   Accounts          ``/accounts``          ``ghcr.io/f5selevin/arcadia-finance-open-banking/accounts:latest``
   Payments          ``/payments``          ``ghcr.io/f5selevin/arcadia-finance-open-banking/payments:latest``
   Swagger UI        ``/swagger/``           ``ghcr.io/f5selevin/arcadia-finance-open-banking/swagger:latest``
   ================  =====================  =====================================================================

Nested paths, such as ``/banks/{bankId}``, are served by the corresponding
API service. Swagger UI displays the complete OpenAPI definition and sends API
requests to the same published domain. Because the GHCR packages are public,
no image pull secret is needed.

1. Confirm that ``kubectl`` is using the downloaded kubeconfig and the expected
   context:

   .. code-block:: console
      export KUBECONFIG="$HOME/Downloads/arcadia-openbanking-vk8s.conf"
      kubectl config current-context
      kubectl get namespaces

2. Set the namespace assigned to you in F5 XC as the current context's default:

   .. code-block:: console
      kubectl config set-context --current --namespace="<namespace>"

3. Create a file named ``openbanking.yaml`` with all four Deployments and
   ClusterIP Services:

   .. code-block:: yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: openbanking-banks
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: openbanking-banks
        template:
          metadata:
            labels:
              app: openbanking-banks
          spec:
            containers:
              - name: banks
                image: ghcr.io/f5selevin/arcadia-finance-open-banking/banks:latest
                imagePullPolicy: Always
                ports:
                  - name: http
                    containerPort: 8080
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: openbanking-banks
      spec:
        selector:
          app: openbanking-banks
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080
      ---
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: openbanking-accounts
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: openbanking-accounts
        template:
          metadata:
            labels:
              app: openbanking-accounts
          spec:
            containers:
              - name: accounts
                image: ghcr.io/f5selevin/arcadia-finance-open-banking/accounts:latest
                imagePullPolicy: Always
                ports:
                  - name: http
                    containerPort: 8080
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: openbanking-accounts
      spec:
        selector:
          app: openbanking-accounts
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080
      ---
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: openbanking-payments
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: openbanking-payments
        template:
          metadata:
            labels:
              app: openbanking-payments
          spec:
            containers:
              - name: payments
                image: ghcr.io/f5selevin/arcadia-finance-open-banking/payments:latest
                imagePullPolicy: Always
                ports:
                  - name: http
                    containerPort: 8080
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: openbanking-payments
      spec:
        selector:
          app: openbanking-payments
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080
      ---
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: openbanking-swagger
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: openbanking-swagger
        template:
          metadata:
            labels:
              app: openbanking-swagger
          spec:
            containers:
              - name: swagger-ui
                image: ghcr.io/f5selevin/arcadia-finance-open-banking/swagger:latest
                imagePullPolicy: Always
                ports:
                  - name: http
                    containerPort: 8080
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: openbanking-swagger
      spec:
        selector:
          app: openbanking-swagger
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080

4. Validate and deploy the manifest:

   .. code-block:: console
      kubectl apply --dry-run=server -f openbanking.yaml
      kubectl apply -f openbanking.yaml

5. Wait for all four Deployments and verify the eight resources:

   .. code-block:: console
      kubectl rollout status deployment/openbanking-banks --timeout=5m
      kubectl rollout status deployment/openbanking-accounts --timeout=5m
      kubectl rollout status deployment/openbanking-payments --timeout=5m
      kubectl rollout status deployment/openbanking-swagger --timeout=5m
      kubectl get deployments,pods,services

   The expected result is one ready pod and one ClusterIP Service for Banks,
   Accounts, Payments, and Swagger UI.

6. If a pod does not become ready, inspect the affected service. For example:

   .. code-block:: console
      kubectl describe pods -l app=openbanking-banks
      kubectl logs deployment/openbanking-banks

   An ``ImagePullBackOff`` status usually means that the image name is invalid
   or the GHCR package is not publicly visible.

.. note::

   F5 XC vK8s is managed and tenant-scoped. Cluster-scoped resources and
   privileged workload settings may be rejected. This manifest uses only
   namespace-scoped resources.
