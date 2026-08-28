Create an F5 XC API token
#########################

Create a short-lived API token that the jump host can use to request the vK8s
kubeconfig through the F5 XC API.

1. In the F5 XC Console, select **Administration**.
2. Go to **Personal Management** > **Credentials** and select **Add Credentials**.
3. Enter ``arcadia-kubeconfig-api`` as the credential name.
4. Select **API Token** as the credential type.
5. Select an expiry date that covers the workshop, and then select **Generate**.
6. Copy the token when it is displayed. XC does not display the token again.

.. warning::

   The API token inherits your user permissions. Treat it as a secret, do not
   save it in source control, and revoke it after the workshop.

.. note::

   Console labels can vary by release. If **Administration** is not shown,
   select **All Workspaces**. You may also need to enable advanced navigation.
