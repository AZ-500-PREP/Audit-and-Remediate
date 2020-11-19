# Enable System Updates Monitoring

## Audit



#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to Azure Security Center blade at https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/.

3. In the navigation panel, choose **Security policy** to access **Policy Management** portal.

4. On the **Policy Management** page, click on the name of the subscription that you want to examine to access the selected subscription configuration settings.

5. On the **Security Policy** page, in the **Compute and Apps** category, check the **System updates should be installed on your machines** setting status. If the configuration setting status is set to **Disabled**, the system updates recommendations are not enabled for the Microsoft Azure virtual machines (VMs) provisioned within the current subscription.

6. Repeat step no. 4 and 5 for each Microsoft Azure subscription available in your account.



#### Using Azure CLI and PowerShell

1. Run **account get-access-token** command (Windows/macOS/Linux) using custom query filters to get the "Monitor System Updates" feature status for the current Azure account subscription:

```
az account get-access-token
	--query "{subscription:subscription,accessToken:accessToken}"
	--out tsv | xargs -L1 bash -c 'curl -X GET -H "Authorization: Bearer $1" -H "Content-Type: application/json" https://management.azure.com/subscriptions/$0/providers/Microsoft.Authorization/policyAssignments/SecurityCenterBuiltIn?api-version=2018-05-01' | jq 'select(.name=="SecurityCenterBuiltIn")'|jq '.properties.parameters.systemUpdatesMonitoringEffect.value'
```



2. The command output should return the requested feature configuration status:

```
"Disabled"
If the command output returns **"Disabled"**, as shown in the example above, the system updates monitoring and recommendations are not enabled for the Azure virtual machines (VMs) available in the selected subscription.
```



3. Repeat step no. 1 and 2 for each Microsoft Azure subscription available in your account.



## Remediation



#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to Azure Security Center blade at https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/.

3. In the navigation panel, choose **Security policy** to access **Policy Management** portal.

4. On the **Policy Management** page, click on the name of the subscription that you want to reconfigure to access the subscription configuration settings.

5. On the **Security Policy** page, click on the **ASC Default (subscription: abcdabcd-1234-1234-1234-abcdabcdabcd)** policy assignment to edit the policy configuration.

6. On the selected policy assignment page, in the **PARAMETERS** section, select **AuditIfNotExists** from **Monitor system updates** dropdown list to enable the system updates monitoring for the Microsoft Azure virtual machines available in the selected Azure subscription.

7. Click **Save** to apply the changes. If successful, the following message should be displayed: **"Updating policy assignment succeeded"**. Once the configuration changes are saved, your Azure subscription will receive system updates recommendations for all the provisioned VMs.

8. If required, repeat steps no. 4 – 7 for other Microsoft Azure cloud subscription available.



#### Using Azure CLI and PowerShell

1. Define the necessary parameters for the **account get-access-token** command, where the **systemUpdatesMonitoringEffect** configuration parameter is enabled. Save the following content to a JSON file named enable-system-updates-monitoring.json and replace the highlighted details, i.e. **<azure-subscription-id>** and **<policy-definition-id>**, with your own Azure account details:



```
{
   "properties":{
      "displayName":"ASC Default (subscription: <azure-subscription-id>)",
      "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/<policy-definition-id>",
      "scope":"/subscriptions/<azure-subscription-id>",
      "parameters":{
         "systemUpdatesMonitoringEffect":{
            "value":"AuditIfNotExists"
         }
      }
   },
   "id":"/subscriptions/<azure-subscription-id>/providers/Microsoft.Authorization/policyAssignments/SecurityCenterBuiltIn",
   "type":"Microsoft.Authorization/policyAssignments",
   "name":"SecurityCenterBuiltIn",
   "location":"eastus"
}
```



2. Run account **get-access-token** command (Windows/macOS/Linux) using the parameters defined at the previous step (i.e. enable-system-updates-monitoring.json file) to enable the system updates monitoring for the Microsoft Azure virtual machines available in the selected Azure subscription (the command request does not produce an output):

```
az account get-access-token
	--query "{subscription:subscription,accessToken:accessToken}"
	--out tsv | xargs -L1 bash -c 'curl -X PUT -H "Authorization: Bearer $1" -H "Content-Type: application/json" https://management.azure.com/subscriptions/$0/providers/Microsoft.Authorization/policyAssignments/SecurityCenterBuiltIn?api-version=2018-05-01 -d@"enable-system-updates-monitoring.json"'
```



3. If successful, the command output should return the updated Azure Security Center policy (truncated):

```
{
   "sku":{
      "name":"A0",
      "tier":"Free"
   },
   "properties":{
      "displayName":"ASC Default (subscription: abcdabcd-1234-1234-1234-abcdabcdabcd)",
      "scope":"/subscriptions/abcdabcd-1234-1234-1234-abcdabcdabcd",

      ...

      "description":"This is the default set of policies monitored by Azure Security Center. It was automatically assigned as part of on-boarding to Security Center. The default assignment contains only audit policies. For more information please visit https://aka.ms/ascpolicies",
   },
   "id":"/subscriptions/abcdabcd-1234-1234-1234-abcdabcdabcd/providers/Microsoft.Authorization/policyAssignments/SecurityCenterBuiltIn",
   "type":"Microsoft.Authorization/policyAssignments",
   "name":"SecurityCenterBuiltIn",
   "location":"eastus"
}
```



4. If required, repeat steps no. 1 – 3 for other Microsoft Azure cloud subscription available.







