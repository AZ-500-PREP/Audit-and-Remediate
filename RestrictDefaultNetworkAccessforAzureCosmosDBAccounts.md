

# Restrict Default Network Access for Azure Cosmos DB Accounts



## Audit

#### Using Azure Portal

1. Sign in to Azure Management Console.

2. Navigate to **All resources** blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. Choose the Azure subscription that you want to access from the **Subscription** filter box.

4. From the **Type** filter box, select **Azure Cosmos DB account** to list all Cosmos DB accounts created in the selected subscription.

5. Click on the name of the Cosmos DB account that you want to examine.

6. In the navigation panel, under **Settings**, select **Firewalls and virtual networks** to access network security configuration page for the selected account.

7. On the **Firewalls and virtual networks** page, check the **Allow access from** network setting configuration. If **Allow access from** is set to **All networks**, all networks, including the Internet, can access the selected Azure Cosmos DB account, therefore the account network access configuration is not compliant.

8. Repeat steps no. 5 – 7 for each Cosmos DB account available in the selected subscription.

9. Repeat steps no. 3 – 8 for each subscription created in your Microsoft Azure cloud account.



#### Using Azure CLI

1. Run **cosmosdb list** command (Windows/macOS/Linux) using custom query filters to list the IDs of all Cosmos DB accounts available in the current Azure subscription:



```
az cosmosdb list
	--query '[*].id'
```



2. The command output should return the requested Azure resource identifiers (IDs):



```
[
"/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/cloud-shell-storage-westeurope/providers/Microsoft.DocumentDB/databaseAccounts/cc-project5-account",
"/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/cloud-shell-storage-westeurope/providers/Microsoft.DocumentDB/databaseAccounts/cc-cosmos-app-account"
]
```



3. Run **cosmosdb show** command (Windows/macOS/Linux) using the name of the Cosmos DB account that you want to examine as identifier parameter and custom query filters to describe the network access configuration implemented for the selected account:



```
az cosmosdb show
	--ids "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/cloud-shell-storage-westeurope/providers/Microsoft.DocumentDB/databaseAccounts/cc-project5-account"
	--query '{"ipRangeFilter":ipRangeFilter,"isVirtualNetworkFilterEnabled":isVirtualNetworkFilterEnabled}'
```



4. The command output should return the requested network access configuration (virtual network and firewall configuration):



```
{
  "ipRangeFilter": "",
  "isVirtualNetworkFilterEnabled": false
}
```


If **cosmosdb show** command output returns **false** for the **"isVirtualNetworkFilterEnabled"** attribute and an empty string (i.e. "") for the **"ipRangeFilter"** attribute, there are no Azure virtual networks and IPs/IP ranges configured, all networks, including the Internet, can access the selected Azure Cosmos DB account, therefore the account network access configuration is not compliant.



5. Repeat step no. 3 and 4 for each Cosmos DB account available within the current subscription.

6. Repeat steps no. 1 – 5 for each subscription created in your Microsoft Azure cloud account.





## Remediation 

### Using Azure Portal

1. Sign in to Azure Management Console.

2. Navigate to **All resources** blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. Choose the Azure subscription that you want to access from the **Subscription** filter box.

4. From the **Type** filter box, select **Key vault** to list all Cosmos DB accounts available in the selected subscription.

5. Click on the name of the Azure Cosmos DB account that you want to reconfigure.

6. In the navigation panel, under **Settings**, select **Firewalls and virtual networks** to access network security configuration page for the selected account.

7. On the **Firewalls and virtual networks** page, choose **Selected networks** under **Allow access from** to show the network security configuration panel for the selected Cosmos DB account.

8. On the configuration panel, perform the following operations:
   1. To secure your Azure Cosmos DB account access with virtual networks, use **+ Add existing virtual network or + Add new virtual network** option available in the **Virtual networks** section to attach an existing virtual network or to create and attach a new one.
   2. To add IPv4 addresses or IPv4 address ranges in order to allow access from a trusted machine on the Internet or from your on-premises network(s), use the configuration controls available under **IPv4 address or CIDR**, in the **Firewall** section.
   3. To configure a network access exception such as a trusted Microsoft service, use the controls available in the **Exception** section.



9. Once the network security (including firewalls and virtual networks) for the selected Azure Cosmos DB account is properly configured, click **Save** to apply the changes. Note that the firewall settings that allow access to the vault resources will remain in effect for up to a minute after saving the new access settings.

10. Repeat steps no. 5 – 9 for each Cosmos DB account available in the selected subscription.

11. Repeat steps no. 3 – 10 for each subscription created in your Microsoft Azure cloud account.







#### Using Azure CLI

1. Configure one or more Cosmos DB account firewall rules in order to grant access from your trusted IP(s) and network(s) only. As example, the following configuration grants access to a specific on-premises network and enables virtual network access, while blocking general Internet traffic. To allow access from your trusted network only, run **cosmosdb update** command (Windows/macOS/Linux) to add a new network firewall rule for a trusted IP address range (e.g. 15.16.17.0/24):



```
az cosmosdb update
	--ids "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/cloud-shell-storage-westeurope/providers/Microsoft.DocumentDB/databaseAccounts/cc-project5-account"
	--ip-range-filter 15.16.17.0/24
	--enable-virtual-network true
```



2. The command output should return the configuration metadata available for the selected Cosmos DB account:



```
{
  "databaseAccountOfferType": "Standard",
  "disableKeyBasedMetadataWriteAccess": false,
  "documentEndpoint": "https://cc-project5-account.documents.azure.com:443/",
  "enableAutomaticFailover": false,
  "enableCassandraConnector": null,
  "enableMultipleWriteLocations": false,

  ...

  "ipRangeFilter": "15.16.17.0/24",
  "isVirtualNetworkFilterEnabled": true,

  ...

  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://cc-project5-account-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "cc-project5-account-db-westeurope",
      "provisioningState": "Succeeded"
    }
  ]
}
```



3. To allow access from a trusted Azure virtual network, run **cosmosdb network-rule add** command (Windows/macOS/Linux) to add a new virtual network to your Azure Cosmos DB account configuration:



```
az cosmosdb network-rule add
	--ids "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/cloud-shell-storage-westeurope/providers/Microsoft.DocumentDB/databaseAccounts/cc-project5-account"
	--virtual-network cc-prod-vnet
	--subnet westeurope-01
```



4. The command output should return the reconfigured Azure Cosmos DB account metadata:



```
{
  "databaseAccountOfferType": "Standard",
  "disableKeyBasedMetadataWriteAccess": false,
  "documentEndpoint": "https://cc-project5-account.documents.azure.com:443/",
  "enableAutomaticFailover": false,
  "enableCassandraConnector": null,
  "enableMultipleWriteLocations": false,

  ...

  "ipRangeFilter": "15.16.17.0/24",
  "isVirtualNetworkFilterEnabled": true,
  "virtualNetworkRules": [
    {
      "id": "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/cloud-shell-storage-westeurope/providers/Microsoft.Network/virtualNetworks/cc-prod-vnet/subnets/westeurope-01",
      "ignoreMissingVnetServiceEndpoint": false,
      "resourceGroup": "cloud-shell-storage-westeurope"
    }
  ],

  ...

  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://cc-project5-account-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "cc-project5-account-db-westeurope",
      "provisioningState": "Succeeded"
    }
  ]
}
```



5. Repeat steps no. 1 – 4 for each Cosmos DB account available within the current subscription.

6. Repeat steps no. 1 – 5 for each subscription created in your Microsoft Azure cloud account.
