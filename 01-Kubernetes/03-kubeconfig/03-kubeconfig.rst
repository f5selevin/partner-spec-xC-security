Generate the vK8s kubeconfig on the jump host
#############################################

Use the API token from the previous section to request a short-lived kubeconfig
directly from F5 XC. The vK8s object and Kubernetes workloads belong to your
assigned ``$$namespace$$`` application namespace. The generated commands save
the kubeconfig on the jump host and set that namespace as the current kubectl
context; no SCP or browser download is required.

1. Enter the required XC API token from the previous section.
2. Enter the vK8s object name you created. The predefined tenant, expiration,
   and assigned namespace are supplied automatically.
3. Select **Generate**.
4. Copy the generated commands and run them on the jump host.

.. react:: CodeGenerator
   :language: console
   :parameters: [{"name":"tenant","title":"XC tenant name","default":"labs-msp-c-secure","readonly":true},{"name":"apiToken","title":"XC API token","type":"text","required":true},{"name":"vk8s","title":"Virtual K8s name","default":"arcadia-finance-vk8s","required":true},{"name":"expiry","title":"Kubeconfig expiry (days)","default":"1","readonly":true}]

   XC_API_TOKEN='$$apiToken$$'
   export HOME="${HOME:-$(getent passwd "$(id -u)" | cut -d: -f6)}"
   umask 077
   mkdir -p "$HOME/.kube"
   response_file="$(mktemp)"
   trap 'rm -f "$response_file"; unset XC_API_TOKEN' EXIT

   curl --fail-with-body --silent --show-error \
     --request POST \
     "https://$$tenant$$.console.ves.volterra.io/api/web/namespaces/system/api_credentials" \
     --header "Authorization: APIToken ${XC_API_TOKEN}" \
     --header "Content-Type: application/json" \
     --data '{
       "namespace": "system",
       "name": "arcadia-vk8s-'"$(date +%s)"'",
       "spec": {
         "type": "KUBE_CONFIG",
         "virtual_k8s_namespace": "$$namespace$$",
         "virtual_k8s_name": "$$vk8s$$"
       },
       "expiration_days": $$expiry$$
     }' > "$response_file"

   jq -er '.data' "$response_file" | base64 --decode > "$HOME/.kube/config"
   chmod 600 "$HOME/.kube/config"
   export KUBECONFIG="$HOME/.kube/config"
   kubectl config set-context --current --namespace="$$namespace$$"
   kubectl config view --minify
   kubectl cluster-info
   kubectl get pods --namespace="$$namespace$$"

.. warning::

   The generated code contains the API token, and the kubeconfig is also a
   credential. Do not save or share the generated code, add either credential
   to source control, or run the commands on a shared terminal. Revoke the API
   token and delete the kubeconfig after the workshop.

.. note::

   The credential API path and request field use ``system`` because XC stores
   personal credential records in its system namespace. The
   ``virtual_k8s_namespace`` field selects the assigned ``$$namespace$$``
   application namespace where the vK8s object was created. Subsequent kubectl
   requests run against that assigned namespace.

.. note::

   The jump host requires outbound HTTPS access to your XC tenant and the
   ``curl``, ``jq``, ``base64``, ``getent``, and ``kubectl`` commands. If the
   shell does not define ``HOME``, the generated commands obtain the current
   user's home directory from the system account database. A ``403`` response
   means the token's user does not have permission to generate the kubeconfig.
