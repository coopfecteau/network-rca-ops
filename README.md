# network-rca-ops

Network change tickets for the network-rca Dynatrace demo. Dynatrace writes here; people approve here.

| Issue label | Opened by | What happens |
|---|---|---|
| `network-remediation` | Dynatrace workflow *Propose network remediation*, when a network problem opens | The **Approve network remediation** Action waits on the protected `network-change` environment. A required reviewer approves it in GitHub; the Action runs the Dynatrace approval workflow; the automation runner in the network applies the fix; Dynatrace comments the verified result and closes the issue. |
| `capacity` | Dynatrace workflow *Network capacity forecast* | Davis forecasts an uplink to cross 85% within 2 hours; the issue names the services whose traffic crosses it and the owning teams. Informational: nothing is changed. |

Issues name the Davis problem by id only; open it in the tenant's Problems app.

## Setup (once)

1. **Dynatrace -> GitHub** (so workflows can open and comment on issues): create a fine-grained
   personal access token for this repository only, with *Issues: read and write*. In Dynatrace,
   add it to the **Credential vault** as a *Token* credential, scope *App Engine*, named
   `[network-rca] GitHub issues`.
2. **GitHub -> Dynatrace** (so the Action can run the approval workflow): in Dynatrace Account
   Management, create an **OAuth client** with scopes `automation:workflows:run` and
   `automation:workflows:read`. Then set the repository secrets:

   ```bash
   gh secret set DT_ENV_URL             -R coopfecteau/network-rca-ops   # https://<env>.apps.dynatrace.com
   gh secret set DT_SSO_URL             -R coopfecteau/network-rca-ops   # https://sso.dynatrace.com (sprint: https://sso-sprint.dynatracelabs.com)
   gh secret set DT_CLIENT_ID           -R coopfecteau/network-rca-ops
   gh secret set DT_CLIENT_SECRET       -R coopfecteau/network-rca-ops
   gh secret set DT_ACCOUNT_URN         -R coopfecteau/network-rca-ops   # urn:dtaccount:<account uuid>
   gh secret set DT_APPROVE_WORKFLOW_ID -R coopfecteau/network-rca-ops   # id of "[network-rca] Approve network remediation"
   ```

The tenant URL and credentials live only in secrets (masked in the public Action logs).
