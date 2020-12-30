# Use Azure Active Directory Admin for PostgreSQL Authentication

## Audit

### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to **All resources** blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. From the **Type** filter box, select **Azure Database for PostgreSQL server** to list only the PostgreSQL database servers provisioned in your Azure account.

4. Click on the name of the PostgreSQL server that you want to examine.

5. In the navigation panel, under **Settings**, select **Active Directory admin** to access the Azure Active Directory (AAD) authentication settings for the selected PostgreSQL database server.

6. On the Active Directory admin configuration page, check the **Active Directory admin** feature status. If the status is currently set to **No Active Directory admin**, there is no Active Directory administrator configured to handle authentication for the selected Azure PostgreSQL database server.

7. Repeat steps no. 4 – 6 for each PostgreSQL database server available in the selected subscription.

8. Repeat steps no. 3 – 7 for each subscription created in your Microsoft Azure cloud account.





## Remediation 

### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to **All resources** blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. From the **Type** filter box, select **Azure Database for PostgreSQL server** to list only the PostgreSQL database servers available in your Azure account.

4. Click on the name of the PostgreSQL database server that you want to reconfigure.

5. In the navigation panel, under **Settings**, select **Active Directory admin** to access the Azure Active Directory (AAD) authentication settings for the selected database server.

6. On the Active Directory admin configuration page, click **Set admin** to initiate the setup process.

7. On the **Active Directory admin** panel, choose the Azure Active Directory (AAD) administrator (or search it by the name and/or email address) that you want to configure for authentication to your Microsoft Azure PostgreSQL database server, then click **Select** to select the chosen AD admin user and return to the configuration page.

8. Click **Save** to apply the configuration changes.

9. Repeat steps no. 4 – 8 for each PostgreSQL database server available within the selected subscription.

10. Repeat steps no. 3 – 9 for each subscription created within your Microsoft Azure cloud account.





