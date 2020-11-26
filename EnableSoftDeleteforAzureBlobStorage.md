# Enable Soft Delete for Azure Blob Storage

## Audit

### Using Azure Portal

01. Sign in to Azure Management Console.

02. Navigate to Azure Storage accounts blade at https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2FStorageAccounts.

03. On the Storage accounts page, select the subscription that you want to examine from the Subscription filter box.

04. Click on the name of the Azure Storage account that you want to examine.

05. In the navigation panel, under Blob service, click Data Protection to access the Soft Delete feature configuration settings.

06. On the Data Protection page, check the Turn on soft delete for blobs configuration setting. If the setting is not checked, the Soft Delete data protection feature is not enabled for the selected Microsoft Azure Storage account.

07. Repeat steps no. 4 – 6 for each storage account available in the selected Azure subscription.

08. Repeat steps no. 3 – 7 for each subscription created within your Microsoft Azure cloud account.


### Using Azure CLI

1. Run **storage account list** command (Windows/macOS/Linux) using custom query filters to describe the identifier for each storage account available in the current Azure subscription:



```
az storage account list
    --query '[*].name'
```



2. The command output should return the requested storage account identifiers (names):



```
[
  "abcd1234abcd1234abcd1234",
  "abcdabcdabcd123412341234"
]
```



3. Run **storage blob service-properties delete-policy show** command (Windows/macOS/Linux) using the name of the storage account that you want to examine as identifier parameter and custom query filters to describe the Soft Delete feature status set for the selected Azure Storage account:



```
az storage blob service-properties delete-policy show
    --account-name abcd1234abcd1234abcd1234
    --query 'enabled'
```



4. The command output should return the requested configuration status (**true** for enabled, **false** for disabled):



```
false
```



If the **storage blob service-properties delete-policy show** command output returns **false**, as shown in the example above, the Soft Delete feature is not enabled for the selected Microsoft Azure Storage account.



5. Repeat step no. 3 and 4 for each storage account provisioned in the current subscription.

6. Repeat steps no. 1 – 5 for each subscription available in your Microsoft Azure cloud account.





## Remediation


#### Using Azure Portal

1. Sign in to Azure Management Console.

2. Navigate to Azure Storage accounts blade at https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2FStorageAccounts.

3. On the **Storage accounts** page, select the Azure subscription that you want to access from the **Subscription** filter box.

4. Click on the name of the Azure Storage account that you want to reconfigure.

5. In the navigation panel, under **Blob service**, click **Data Protection** to access the Soft Delete feature configuration settings.

6. On the **Data Protection** page, perform the following operations:
   1. Check the **Turn on soft delete for blobs** configuration setting to enable the Soft Delete data protection feature for the selected storage account.
   2. Enter the [optimal](https://www.cloudconformity.com/knowledge-base/azure/StorageAccounts/sufficient-soft-delete-retention-period.html) data retention period in the **Keep deleted blobs for (in days)** box. The retention period represents the amount of time that soft deleted data is stored and available for recovery.
   3. Click **Save** to apply the configuration changes.

7. Repeat steps no. 4 – 6 for each storage account available in the selected Azure subscription.

8. Repeat steps no. 3 – 7 for each subscription created within your Microsoft Azure cloud account.





#### Using Azure CLI

1. Run **storage blob service-properties delete-policy update** command (Windows/macOS/Linux) using the name of the storage account that you want to reconfigure as identifier parameter to enable the Soft Delete feature for the selected Microsoft Azure Storage account, and configure it to retain soft deleted data for a sufficient number of days (in this case 30 days):

```
az storage blob service-properties delete-policy update
    --account-name abcd1234abcd1234abcd1234
    --enable true
    --days-retained 30
```



2. The command output should return the configuration metadata for the **storage blob service-properties delete-policy update** command request:

```
{
  "enabled": true,
  "days": 30
}
```

3. Repeat step no. 1 and 2 for each storage account available within the current subscription.

4. Repeat steps no. 1 – 3 for each subscription available in your Microsoft Azure cloud account.




























