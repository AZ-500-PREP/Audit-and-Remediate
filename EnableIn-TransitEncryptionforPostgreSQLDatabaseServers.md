# Enable In-Transit Encryption for PostgreSQL Database Servers


## Audit

#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to All resources blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. From the **Type** filter box, select **Azure Database for PostgreSQL server** to list the PostgreSQL servers available within your Azure account.

4. Click on the name of the PostgreSQL database server that you want to examine.

5. In the navigation panel, under **Settings**, select **Connection security** to access the connection security settings for the selected PostgreSQL server.

6. On the **Connection security** configuration page, in the **SSL settings** section, check the **Enforce SSL connection** status. If the setting status is set to **DISABLED**, in-transit encryption is not enabled for the selected Microsoft Azure PostgreSQL server.

7. Repeat steps no. 4 – 6 for each PostgreSQL database server available in the current Azure subscription.

8. Repeat steps no. 3 – 7 for each subscription created in your Microsoft Azure cloud account.

#### Using Azure PowerShell

1. Run **postgres server list** command (Windows/macOS/Linux) using custom query filters to list the names of all PostgreSQL database servers (and the name of their associated resource groups) available in the current Azure subscription:



```bash
az postgres server list
	--output table
	--query '[*].{name:name, resourceGroup:resourceGroup}'
```



2. The command output should return a table with requested PostgreSQL server information:



```bash
Name                  ResourceGroup
--------------------  ------------------------------
cc-production-server  cloud-shell-storage-westeurope
cc-postgresql-server  cloud-shell-storage-westeurope
```



3. Run **postgres server show** command (Windows/macOS/Linux) using the name of the Azure PostgreSQL server that you want to examine as identifier parameter and custom query filters to obtain the "Enforce SSL connection" setting status for the selected database server:



```bash
az postgres server show
	--name "cc-production-server"
	--resource-group "cloud-shell-storage-westeurope"
	--query sslEnforcement
```



4. The command output should return the requested configuration setting status:



```bash
"Disabled"
```



> If  **postgres server show** command output returns **"Disabled"**, as shown in the example above, the encryption in transit using SSL is not enabled for the selected Microsoft Azure PostgreSQL database server.



5. Repeat step no. 3 and 4 for each Azure PostgreSQL server provisioned in the selected subscription.

6. Repeat steps no. 1 – 5 for each subscription available within your Microsoft Azure cloud account.



## Remediation

#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to All resources blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. From the **Type** filter box, select **Azure Database for PostgreSQL server** to list the PostgreSQL servers available in your Azure account.

4. Click on the name of the PostgreSQL server that you want to reconfigure (see Audit section part I to identify the right PostgreSQL resource).

5. In the navigation panel, under **Settings**, select **Connection security** to access the connection security settings for the selected PostgreSQL database server.

6. On the **Connection security** configuration page, in the **SSL settings** section, select **ENABLED** next to **Enforce SSL connection** setting to enable in-transit encryption for the selected Azure PostgreSQL database server.

7. Click **Save** to apply the configuration changes.

8. Repeat steps no. 4 – 7 for each PostgreSQL database server available within the selected subscription.

9. Repeat steps no. 3 – 8 for each subscription created in your Microsoft Azure cloud account.



#### Using Azure CLI and PowerShell

1. Run **postgres server update** command (Windows/macOS/Linux) using the name of the Azure PostgreSQL server that you want to reconfigure as identifier parameter (see Audit section part II to identify the right Azure resource) to enable in-transit encryption for the selected database server by setting the **--ssl-enforcement** parameter to **Enabled**:



```bash
az postgres server update
	--name "cc-production-server"
	--resource-group "cloud-shell-storage-westeurope"
	--ssl-enforcement Enabled
```



2. The command output should return the metadata for the reconfigured Azure PostgreSQL server:



```bash
{
  "earliestRestoreDate": "2019-07-24T11:28:45.067000+00:00",
  "fullyQualifiedDomainName": "cc-production-server.postgres.database.azure.com",
  "location": "westeurope",
  "name": "cc-production-server",
  "replicaCapacity": 5,
  "replicationRole": "None",
  "resourceGroup": "cloud-shell-storage-westeurope",
 
  ...bash
 
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 30,
    "geoRedundantBackup": "Disabled",
    "storageAutoGrow": "Disabled",
    "storageAutogrow": null,
    "storageMb": 5120
  },
  "tags": {},
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "10"
}
```



3. Repeat step no. 1 and 2 for each PostgreSQL database server available in the selected subscription.

4. Repeat steps no. 1 – 3 for each subscription created within your Microsoft Azure cloud account.





## References

### Azure Official Documentation

- [Configure SSL connectivity in Azure Database for PostgreSQL - Single Server](https://docs.microsoft.com/en-us/azure/postgresql/concepts-ssl-connection-security)
- [Quickstart: Create an Azure Database for PostgreSQL server in the Azure portal](https://docs.microsoft.com/en-us/azure/postgresql/quickstart-create-server-database-portal)
- [CIS Microsoft Azure Foundations](https://azure.microsoft.com/mediahandler/files/resourcefiles/cis-microsoft-azure-foundations-security-benchmark/CIS_Microsoft_Azure_Foundations_Benchmark_v1.0.0.pdf)

### Azure Command Line Interface (CLI) Documentation

- [az postgres server](https://docs.microsoft.com/en-us/cli/azure/postgres/server?view=azure-cli-latest)
- [az postgres server list](https://docs.microsoft.com/en-us/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-list)
- [az postgres server show](https://docs.microsoft.com/en-us/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-show)
- [az postgres server update](https://docs.microsoft.com/en-us/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-update)









