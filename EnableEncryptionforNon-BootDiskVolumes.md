# Enable Encryption for Non-Boot Disk Volumes 



## Audit 

#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to **All resources** blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. Choose the Azure subscription that you want to access from the **Subscription** filter box.

4. From the **Type** filter box, select **Virtual machine** to show only the virtual machines (VMs) available in the selected subscription.

5. Click on the name of the virtual machine that you want to examine.

6. In the navigation panel, under **Settings**, select **Disks** to view the disk volumes attached to the selected Azure VM.

7. On the **Disks** overview page, under **Data disks**, check the disk volume encryption status, available in the **ENCRYPTION** column, for each data volume attached. If the encryption status is set to **Not enabled**, the non-boot (data) volumes attached to the selected Microsoft Azure virtual machine (VM) are not encrypted.

8. Repeat steps no. 4 – 7 for each Azure virtual machine available in the selected subscription.

9. Repeat steps no. 3 – 8 for each subscription created in your Microsoft Azure cloud account.





#### Using Azure CLI

1. Run **vm list** command (Windows/macOS/Linux) using custom query filters to list the ID of each virtual machine (VM) provisioned in the current Azure subscription:

```
az vm list
   --query '[*].id'
```

2. The command output should return the requested VM server identifiers (IDs):

```
[
"/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/CLOUD-SHELL-STORAGE-WESTEUROPE/providers/Microsoft.Compute/virtualMachines/cc-warehouse-app-server", "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/CLOUD-SHELL-STORAGE-WESTEUROPE/providers/Microsoft.Compute/virtualMachines/cc-internal-app-server"
]
```

3. Run **vm encryption** show command (Windows/macOS/Linux) using the ID of the virtual machine that you want to examine as identifier parameter to obtain the encryption status set for the non-boot (data) disk volumes attached to the selected Azure VM:

```
az vm encryption show
	--ids "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/CLOUD-SHELL-STORAGE-WESTEUROPE/providers/Microsoft.Compute/virtualMachines/cc-warehouse-app-server"
	--query 'dataDisk'
```

4. The command output should return the requested VM non-boot volume encryption status:

```
Azure Disk Encryption is not enabled
```

If the **vm encryption show** command output returns the following message: **Azure Disk Encryption is not enabled**, the non-boot disk volumes attached to the selected Microsoft Azure virtual machine (VM) are not configured to encrypt data at rest.



5. Repeat step no. 3 and 4 for every Azure virtual machine launched within the current subscription.

6. Repeat steps no. 1 – 5 for each subscription available in your Microsoft Azure cloud account.



## Remediation

#### Using Azure CLI

1. Run **keyvault create** command (Windows/macOS/Linux) to create the Microsoft Azure Key Vault where the encryption key required for data disk volume encryption will be stored. Make sure that you set the **--enabled-for-disk-encryption** parameter to **true** to enable VM disk encryption support:

```
az keyvault create
	--name cc-disk-encryption-vault
	--resource-group cloud-shell-storage-westeurope
	--location westeurope
	--enable-soft-delete true
	--enable-purge-protection true
	--enabled-for-disk-encryption true
```



2. The command output should return the configuration metadata for the newly created Azure Key Vault:

```
{
  "id": "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/cloud-shell-storage-westeurope/providers/Microsoft.KeyVault/vaults/cc-disk-encryption-vault",
  "location": "westeurope",
  "name": "cc-disk-encryption-vault",
  "properties": {
    "accessPolicies": [
      {
        "applicationId": null,
        "objectId": "1234abcd-1234-abcd-1234-abcd1234abcd",
        "permissions": {
          "certificates": [
            "get",
            "list",
            "delete",
            "create",
            "import",
            "update",
            "managecontacts",
            "getissuers",
            "listissuers",
            "setissuers",
            "deleteissuers",
            "manageissuers",
            "recover"
          ],
          "keys": [
            "get",
            "create",
            "delete",
            "list",
            "update",
            "import",
            "backup",
            "restore",
            "recover"
          ],
          "secrets": [
            "get",
            "list",
            "set",
            "delete",
            "backup",
            "restore",
            "recover"
          ],
          "storage": [
            "get",
            "list",
            "delete",
            "set",
            "update",
            "regeneratekey",
            "setsas",
            "listsas",
            "getsas",
            "deletesas"
          ]
        },
        "tenantId": "1234abcd-1234-abcd-1234-abcd1234abcd"
      }
    ],
    "createMode": null,
    "enablePurgeProtection": true,
    "enableSoftDelete": true,
    "enabledForDeployment": false,
    "enabledForDiskEncryption": true,
    "enabledForTemplateDeployment": null,
    "networkAcls": null,
    "provisioningState": "Succeeded",
    "sku": {
      "name": "standard"
    },
    "tenantId": "1234abcd-1234-abcd-1234-abcd1234abcd",
    "vaultUri": "https://cc-disk-encryption-vault.vault.azure.net/"
  },
  "resourceGroup": "cloud-shell-storage-westeurope",
  "tags": {},
  "type": "Microsoft.KeyVault/vaults"
}
```

3. Run **vm encryption enable** command (Windows/macOS/Linux) using the ID of the Azure virtual machine that you want to reconfigure as identifier parameter (see Audit section part II to identify the right resource) to enable encryption at rest for all the non-boot (data) disk volumes attached to the selected Azure virtual machine (VM):

```
az vm encryption enable
	--ids "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/CLOUD-SHELL-STORAGE-WESTEUROPE/providers/Microsoft.Compute/virtualMachines/cc-warehouse-app-server"
	--disk-encryption-keyvault cc-disk-encryption-vault
	--volume-type DATA
```

4. If successful, the command output should return a confirmation message, such as:

```
The encryption request was accepted. Please use 'show' command to monitor the progress.
```

5. Run again **vm encryption show** command (Windows/macOS/Linux) using the ID of the reconfigured virtual machine (VM) as identifier parameter to confirm the encryption process completion, by listing the encryption configuration metadata available for the non-boot disk volumes encrypted at the previous steps:

```
az vm encryption show
	--ids "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/CLOUD-SHELL-STORAGE-WESTEUROPE/providers/Microsoft.Compute/virtualMachines/cc-warehouse-app-server"
```

6. The command output should return the disk volume encryption configuration information:

```
{
  "disks": [
    {
      "encryptionSettings": [
        {
          "diskEncryptionKey": {
            "sourceVault": {
              "id": "/subscriptions/abcdabcd-1234-abcd-1234-abcd1234abcd/resourceGroups/CLOUD-SHELL-STORAGE-WESTEUROPE/providers/Microsoft.Compute/virtualMachines/cc-warehouse-app-server"
            }
          },
          "enabled": true,
          "keyEncryptionKey": null
        }
      ],
      "name": "cc-warehouse-app_DataDisk_0",
      "statuses": [
        {
          "code": "EncryptionState/encrypted",
          "displayStatus": "Encryption is enabled on disk",
          "level": "Info",
          "message": null,
          "time": null
        }
      ]
    }
  ],
  "status": [
    {
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": "Encryption succeeded for data volumes",
      "time": null
    }
  ],
  "substatus": [
    {
      "code": "ComponentStatus/Microsoft.Azure.Security.AzureDiskEncryptionForLinux/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": "{\"os\": \"Encrypted\", \"data\": \"EncryptionInProgress\"}",
      "time": null
    }
  ]
}
```

7. Repeat steps no. 3 – 6 for every Azure virtual machine (VM) available within the current subscription.

8. Repeat steps no. 1 – 7 for each subscription created in your Microsoft Azure cloud account.




