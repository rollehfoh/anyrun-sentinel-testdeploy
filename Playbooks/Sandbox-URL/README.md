# ANY.RUN — Sandbox URL

![ANY.RUN](https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/Logos/ANYRUN.svg)

## Summary

When a Microsoft Sentinel incident is created, this playbook:

- Extracts URL entities from the incident.
- Submits each URL for analysis in ANY.RUN Sandbox using the ANY.RUN API.
- Adds a comment to the incident with the analysis verdict, threat score, malware tags, IOC table, and a link to the analysis session.

## Prerequisites

Shared prerequisites (Key Vault secret `ANYRUN-APIKey`, App Registration, API permissions, and role assignments) are described in the [solution README](../../README.md).

Additional requirements for this playbook:

- Microsoft Sentinel incidents must contain URL entities.
- The playbook uses a Key Vault connection to retrieve the ANY.RUN API key. The secret must be named `ANYRUN-APIKey`.

## Deployment

Click below to deploy Azure Logic App with **Flex Consumption plan**

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzure-Sentinel%2Fmaster%2FSolutions%2FANYRUN%2FPlaybooks%2FSandbox-URL%2Fazuredeploy.json)

Enter the parameters required for deploying the Logic App and click **Review + create**.

![logic_app_deployment](Images/1_001.png)

Description of the required parameters:

| Parameter | Description |
|-----------|-------------|
| PlaybookName | Logic App name (default `ANYRUN-Sandbox-URL`). |
| Azure Tenant ID | Tenant ID for authentication in connections. |
| Azure Client ID | Client ID of the App Registration created for the solution. |

## Post-deployment

### Authorize API connections

After deployment, open the Logic App and authorize the API connections created by the template:

- **Microsoft Sentinel**
- **Key Vault**

Edit each connection, sign in with an account that has the required permissions, and save.

## Playbook steps

1. The playbook is triggered when a Microsoft Sentinel incident is created.
2. It retrieves URL entities from the incident.
3. It obtains the ANY.RUN API key from Key Vault.
4. For each URL it sends an HTTP request to the ANY.RUN API to start analysis.
5. It monitors the analysis until completion, then retrieves the report and IOCs.
6. It adds a comment to the incident containing the verdict, threat score, tags, IOC table, and a link to the analysis session.
7. Detected IOCs are uploaded to the Microsoft Sentinel Threat Intelligence portal.

The URL analysis parameters are defined in the HTTP action that starts the analysis. You can adjust them (timeout, browser, OS, interactivity, etc.) in the Logic App designer or code view. See the [ANY.RUN API documentation](https://any.run/api/docs) for the full list of parameters.
