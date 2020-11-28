# Disable Anonymous Access to Blob Containers



## Audit

#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to Azure Storage accounts blade at https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2FStorageAccounts.

3. Click on the name (link) of the Azure Storage account that you want to examine.

4. In the navigation panel, under **Blob service**, click **Blobs** to access the blob containers provisioned in your storage account.

5. On the **Blobs** page, choose the container that you want to examine and check the configuration value available in the **PUBLIC ACCESS LEVEL** column. If the configuration value is set to **Container**, all container and blob data can be read by anonymous request, therefore the anonymous access to the selected Azure Storage blob container is not disabled.

6. Repeat step no. 5 for each blob container provisioned in the selected storage account.

7. Repeat steps no. 3 – 6 for each storage account available within the selected subscription.

8. Repeat steps no. 3 – 7 for each subscription created in your Microsoft Azure cloud account.



#### Using Azure CLI and PowerShell

01.Run **storage account list** command (Windows/macOS/Linux) using custom query filters to describe the identifier for each storage account available in the current Azure subscription:

```
az storage account list 
	--query '[*].name'
```



2. The command output should return the requested storage account names:

```
[
  "abcdabcdabcd123412341234",  
  "abcd1234abcd1234abcd1234"
]
```



3. Run **storage container list** command (Windows/macOS/Linux) using the name of the storage account that you want to examine as identifier parameter and custom query filters to list the containers available in the selected storage account:

```
az storage container list
	--account-name abcdabcdabcd123412341234
	--query '[*].name'
```



4. The command output should return the name of the blob containers within the specified storage account:

```
[
  "cc-project5-container",
  "cc-staging-container"
]
```



5. Run **storage container show** command (Windows/macOS/Linux) using the name of the blob container that you want to examine as identifier parameter to expose the public access level set for the selected container:

```
az storage container show
	--name cc-blob-container
	--account-name abcdabcdabcd123412341234
	--query 'properties.publicAccess'
```



6. The command output should return the name of the configured public access level. There are three levels of public access: Private (no anonymous access), Blob (anonymous read access for blobs only) and Container (anonymous read access for containers and blobs):

```
"container"
```


If the **storage container show** command output returns **"container"**, as shown in the example above, the data available on the selected blob container can be read by anonymous request, therefore the anonymous access to the selected Azure Storage blob container is not disabled.



7. Repeat step no. 5 and 6 for each container provisioned in the selected storage account.

8. Repeat steps no. 3 – 7 for each storage account available within the selected subscription.

9. Repeat steps no. 1 – 8 for each subscription created in your Microsoft Azure cloud account.



## Remediation 

#### Using Azure Console

01.Sign in to Azure Management Console.

2. Navigate to Azure Storage accounts blade at https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2FStorageAccounts.

3. Click on the name (link) of the Azure Storage account that you want to access.

4. In the navigation panel, under **Blob service**, click **Blobs** to access the blob containers provisioned in your storage account.

5. On the **Blobs** page, select the container that you want to reconfigure (see Audit section part I to identify the right resource), then click **Change access level** button from the blade top menu.

6. On the **Change access level** configuration panel, select **Private (no anonymous access)** option from the **Public access level** dropdown list to disable anonymous access for the selected blob container. Click **Ok** to confirm the change.

7. Repeat step no. 5 and 6 for all publicly accessible containers available in the selected storage account.

8. Repeat steps no. 3 – 7 for each storage account available within the selected subscription.

9. Repeat steps no. 3 – 8 for each subscription created in your Microsoft Azure cloud account.



#### Using Azure CLI and PowerShell

1. Run **storage container set-permission** command (Windows/macOS/Linux) using the name of the blob container that you want to reconfigure as identifier parameter  to disable anonymous access to the selected blob container by setting the "Public access level" configuration option to "Private (no anonymous access)":



```
az storage container set-permission
	--name cc-project5-container
	--account-name abcdabcdabcd123412341234
	--public-access off
```



2. Repeat step no. 1 for all publicly accessible containers available in the selected storage account.

3. Repeat step no. 1 and 2 for each storage account available in the current Azure subscription.

4. Repeat step no. 1 – 3 for each subscription created in your Microsoft Azure cloud account.
