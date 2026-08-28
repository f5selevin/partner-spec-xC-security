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

Continue in the same jump-host terminal used in the previous section. The
``KUBECONFIG`` environment variable, vK8s connection, and ``$$namespace$$``
current-context namespace are already configured and verified.

1. Create and open a file named ``finance.yaml``:

   .. code-block:: console

      touch finance.yaml
      nano finance.yaml

   Paste the following manifest, which defines all four Deployments and
   ClusterIP Services:

   .. code-block:: yaml

      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: finance-banks
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: finance-banks
        template:
          metadata:
            labels:
              app: finance-banks
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
        name: finance-banks
      spec:
        selector:
          app: finance-banks
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080
      ---
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: finance-accounts
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: finance-accounts
        template:
          metadata:
            labels:
              app: finance-accounts
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
        name: finance-accounts
      spec:
        selector:
          app: finance-accounts
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080
      ---
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: finance-payments
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: finance-payments
        template:
          metadata:
            labels:
              app: finance-payments
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
        name: finance-payments
      spec:
        selector:
          app: finance-payments
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080
      ---
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: finance-swagger
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: finance-swagger
        template:
          metadata:
            labels:
              app: finance-swagger
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
        name: finance-swagger
      spec:
        selector:
          app: finance-swagger
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 8080

   After pasting the manifest, press ``Ctrl+X``, then ``Y``, and then ``Enter``
   to save the changes and exit nano.

2. Validate and deploy the manifest:

   .. code-block:: console
      kubectl apply --dry-run=server -f finance.yaml
      kubectl apply -f finance.yaml

3. Wait for all four Deployments and verify the eight resources:

   .. code-block:: console
      kubectl wait --for=condition=Available deployment --all --timeout=5m
      kubectl get deployments,pods,services

   The expected result is one ready pod and one ClusterIP Service for Banks,
   Accounts, Payments, and Swagger UI.

4. If a pod does not become ready, inspect the affected service. For example:

   .. code-block:: console
      kubectl describe pods -l app=finance-banks
      kubectl logs deployment/finance-banks

   An ``ImagePullBackOff`` status usually means that the image name is invalid
   or the GHCR package is not publicly visible.

.. note::

   If you opened a new terminal after completing the previous section, run
   ``export KUBECONFIG="$HOME/.kube/config"`` once before applying the manifest.
   The namespace is stored in the kubeconfig context and does not need to be set
   again.

.. note::

   F5 XC vK8s is managed and tenant-scoped. Cluster-scoped resources and
   privileged workload settings may be rejected. This manifest uses only
   namespace-scoped resources.
