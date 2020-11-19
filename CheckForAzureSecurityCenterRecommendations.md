# Check for Azure Security Center Recommendations

## Audit

#### Using Azure Portal

1. Sign in to Azure Management Portal.

2. Navigate to Azure Security Center blade at https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade.

3. In the left navigation panel, click **Overview**, select **Subscriptions**, and choose the Azure subscription that you want to examine from the **Default subscription filter** dropdown menu.

4. In the navigation panel, under **RESOURCE SECURITY HYGIENE**, select **Recommendations** to access the list with the security recommendations generated for the selected subscription.

5. On the **Recommendations** page, switch off **Group by controls** setting to disable grouping, and click on the active recommendation that you want to examine.

6. Analyze the selected Azure Security Center recommendation by verifying the following attributes:
   1. **Description** – an explicit description of the security issue found.
   2. **General Information** – describes the level of user impact after implementing the suggested remediation steps and the level of the implementation effort for the recommended remediation process.
   3. **Remediation steps** – step-by-step instructions on how to implement the selected Azure Security Center recommendation in order to fix the issue.
   4. **Affected resources** – the identifier(s) of the impacted Azure cloud resource(s).



7. Follow the instructions outlined at the previous step, in the **Remediation steps** section, to implement the recommended fix (see Remediation/Resolution section).

8. Repeat steps no. 5 – 7 for each security recommendation identified within the selected Azure subscription.

9. Repeat steps no. 3 – 8 for each subscription created within your Microsoft Azure cloud account.



#### Using Azure CLI and PowerShell

1. Run **account list** command (Windows/macOS/Linux) using custom query filters to list the identifiers (IDs) of all the cloud subscriptions available in your Azure cloud account:

```
az account list
	--query '[*].id'
```



2. The command output should return the requested Azure subscription IDs:

```
[
  "abcdabcd-1234-abcd-1234-abcdabcdabcd",
  "abcd1234-1234-abcd-1234-abcd1234abcd"
]
```



3. Run **security task list** command (Windows/macOS/Linux) using the ID of the Azure cloud subscription that you want to examine as identifier parameter and custom output filters, to list the name of each security task (recommendation) generated for the selected subscription:

```
az security task list
	--subscription "abcdabcd-1234-abcd-1234-abcdabcdabcd"
	--query '[*].name'
```



4. The command output should return the requested recommendation name(s):

```
[
  "abcd1234-abcd-1234-abcd-abcd1234abcd",
  "12341234-abcd-1234-abcd-123412341234",
  "abcd1234-1234-abcd-1234-abcd1234abcd"
]
```



5. Run **security task show** command (Windows/macOS/Linux) using the name of the Azure Security Center recommendation that you want to examine as identifier parameter, to describe the selected security task (recommendation) details:

```
az security task show
	--name "abcd1234-abcd-1234-abcd-abcd1234abcd"
```



6. The command output should return the requested information (JSON format):

```
{
  "creationTimeUtc": "2020-06-02T15:32:18.602725+00:00",
  "id": "/subscriptions/abcdabcd-1234-abcd-1234-abcdabcdabcd/resourceGroups//providers/Microsoft.Security/locations/centralus/tasks/abcd1234-1234-abcd-1234-abcd1234abcd",
  "lastStateChangeTimeUtc": "2020-06-02T15:32:18.602725+00:00",
  "name": "abcd1234-abcd-abcd-abcd-abcd1234abcd",
  "resourceGroup": "",
  "securityTaskParameters": {
    "assessmentKey": "abcd1234-1234-abcd-1234-abcd1234abcd",
    "category": "IdentityAndAccess",
    "details": [],
    "name": "GenericSecurityTask",
    "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/abcd1234-1234-abcd-1234-abcd1234abcd",
    "policyName": "There should be more than one owner assigned to your subscription",
    "resourceId": "/subscriptions/abcdabcd-1234-abcd-1234-abcdabcdabcd",
    "resourceType": "Subscription",
    "severity": "High",
    "uniqueKey": "/subscriptions/abcd1234-1234-abcd-1234-abcd1234abcd_abcd1234-1234-abcd-1234-abcd1234abcd"
  },
  "state": "Active",
  "subState": "NA",
  "type": "Microsoft.Security/locations/centralus/tasks"
}
```



7. Analyze the information returned at the previous step by checking the following attributes:
   1. **"state"** – the status of the selected security recommendation (e.g. active).
   2. **"securityTaskParameters.name"** – the generic name of the security task (i.e. recommendation).
   3. **"securityTaskParameters.policyName"** – the name of the selected Security Center recommendation policy which is also a concise description of the security issue identified.
   4. **"securityTaskParameters.severity"** – the severity level of the selected security task.
   5. **"securityTaskParameters.resourceType"** – the type of the impacted Azure cloud resource.
   6. **"securityTaskParameters.resourceId"** – the ID of the impacted cloud resource.

8. Based on the information found at the previous steps, follow the instructions outlined in the Remediation/Resolution section to implement the recommended security fix.

9. Repeat step no. 5 – 8 for each security recommendation available within the selected Azure subscription.

10. Repeat steps no. 3 – 9 for each subscription available in your Microsoft Azure cloud account.



## Remediation

#### Using Azure Portal

1. Sign in to Azure Management Portal as an account owner.

2. Navigate to **Subscriptions** blade at https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade.

3. Click on the name of the Azure subscription that you want to access.

4. In the navigation panel, choose **Access control (IAM)**, then select **Role assignments** to access the list with the role assignments available for the selected subscription.

5. On the **Role assignments** page, click **Add** and **Add role assignment** to open the **Add role assignment** configuration panel.

6. On the **Add role assignment** panel, perform the following commands:
   1. From the **Role** dropdown list, select **Owner**. The Owner role gives the specified user full administrator access to all Azure resources available in the subscription, including the permission to grant access to others.
   2. From **Assign access to** dropdown list, select **Azure AD user, group, or service principal** option as the type of security principal to assign the role to.
   3. From the **Select** list, choose the user that you want to assign the selected role to. If you cannot find the principal in the list, use the **Select** search box to search the directory for display names or email addresses.
   4. Click **Save** to assign the selected role. Once the role assignment is done, the selected Microsoft Azure account subscription will have 2 active subscription owners.



7. Repeat steps no. 3 – 6 to implement the specified Security Center recommendation for other subscriptions available in your Microsoft Azure cloud account.



#### Using Azure CLI and PowerShell

1. Run **role assignment create** command (Windows/macOS/Linux) using the ID of the Azure cloud subscription that you want to reconfigure as identifier parameter (see Audit section part I to identify the appropriate subscription), to create a new Owner role assignment for an Azure user with the name "johndoe@az500.onmicrosoft.com", at the selected subscription level. Once the role assignment is done, the selected cloud account subscription will have two active subscription owners:

```powershell
az role assignment create
	--role "Owner"
	--assignee johndoe@az500.onmicrosoft.com
	--subscription abcdabcd-1234-abcd-1234-abcdabcdabcd
```



2. The command output should return the new role assignment metadata:

```powershell
{
  "canDelegate": null,
  "id": "/subscriptions/abcdabcd-1234-abcd-1234-abcdabcdabcd/providers/Microsoft.Authorization/roleAssignments/1234abcd-1234-abcd-1234-abcd1234abcd",
  "name": "1234abcd-1234-abcd-1234-abcd1234abcd",
  "principalId": "1234abcd-abcd-abcd-abcd-abcd1234abcd",
  "principalType": "User",
  "roleDefinitionId": "/subscriptions/abcdabcd-1234-abcd-1234-abcdabcdabcd/providers/Microsoft.Authorization/roleDefinitions/1234abcd-abcd-abcd-abcd-abcd1234abcd",
  "scope": "/subscriptions/abcdabcd-1234-abcd-1234-abcdabcdabcd",
  "type": "Microsoft.Authorization/roleAssignments"
}
```



3. Repeat step no. 1 and 2 to implement the selected Security Center recommendation for other subscriptions created within your Microsoft Azure cloud account.



