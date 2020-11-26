# Allow Shared Access Signature Tokens Over HTTPS Only

## Audit

#### Manual Verification

1. Locate the Shared Access Signature (SAS) token defined within the SAS URL provided to your storage account clients. The SAS token starts with a question mark, followed by a set of various parameters, e.g. `?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-06-11T10:25:43Z&st=2019-06-11T11:25:43Z &spr=https,http&sig=abcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcdabcd`. Identify the communication protocol used for the verified token, defined as value for the spr parameter, for example spr=https,http. If the spr parameter is not set to HTTPS only (i.e. spr=https), the selected Shared Access Signature (SAS) token's configuration is not compliant.

2. Repeat step no. 1 for each Shared Access Signature (SAS) URL created for the current storage account.

3. Repeat step no. 1 and 2 for each storage account available within the selected subscription.

4. Repeat steps no. 1 – 3 for each subscription created in your Microsoft Azure cloud account.



## Remediation



#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to Azure Storage accounts blade at https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2FStorageAccounts.

3. Click on the name of the storage account that holds the SAS token that you want to regenerate.

4. In the navigation panel, under **Settings**, choose **Shared access signature** to access the SAS generator.

5. On the **Shared access signature** generator page, perform the following actions to create your new SAS token:
      1. From **Allowed services**, select the Azure Storage services accessible with the Shared Access Signature.
      2. From the **Allowed resource types** section, select the storage resource types accessible with the SAS.
      3. From **Allowed permissions**, choose the permissions required for the account SAS. Permissions are valid only if they match the specified allowed resource type, otherwise these permissions are ignored.
      4. Use the **Start** and **End** date and time picker controls from the **Start and expiry date/time** section to configure the start and the end date/time during which the account SAS is valid. Cloud Conformity strongly recommends a SAS validity period no longer that an hour.
      5. In the **Allowed IP addresses** box, enter the client IP address or range of IP addresses from which to accept access requests.
      6. From **Allowed protocols**, select **HTTPS only** to allow access requests over HTTPS protocol only.
      7. From **Signing key**, select the access key used to authenticate the requests. If the selected access key is regenerated over the SAS lifetime, the SAS token will also need to be regenerated. This action will not interrupt access to disks from your Azure virtual machines.
      8. Click **Generate SAS and connection string** to create your new Azure Shared Access Signature (SAS).



6. Replace the Shared Access Signature (SAS) token defined within the SAS URL(s) provided to your storage account clients with the compliant token generated at the previous step (e.g. `?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-06-11T12:33:40Z&st=2019-06-11T13:33:40Z&spr=https&sig=aaaabbbbccccddddaaaabbbbccccddddaaaabbbbccccdddd)`, available in the SAS token box.

7. If required, repeat step no. 5 and 6 to generate new Shared Access Signature (SAS) tokens.

8. Repeat steps no. 3 – 7 for each storage account available in the current Azure subscription.

9. Repeat steps no. 3 – 8 for each subscription created in your Microsoft Azure cloud account.





#### Using Azure CLI and PowerShell

1. Define the Shared Access Signature (SAS) validity period (the recommended value for this parameter is 1 hour):

```
end=`date -d "60 minutes" '+%Y-%m-%dT%H:%MZ'`
```



2. Run **storage account generate-sas** command (Windows/macOS/Linux) using the name of the storage account that utilize the non-compliant SAS token as identifier parameter, to generate a new Shared Access Signature (SAS) for Blob, File, Queue and Table Azure Storage services, with a validity period of one hour, that allows access requests over HTTPS protocol only:

```
az storage account generate-sas
	--permissions cdlruwap
	--account-name aaaabbbbccccddddaaaabbbb
	--services bfqt
	--resource-types sco
	--https-only
	--expiry $end -otsv
```



3. The command output should return the parameters for the new Shared Access Signature:

```
se=2019-06-11T17%3A23Z&sp=rwdlacup&sv=2018-03-28&ss=qt&srt=sco&sig=abcabc/abcd1234abcd1234abcd1234abcd1234abcd1234
```



4. If required, repeat steps no. 1 – 3 to generate new Shared Access Signature (SAS) tokens.

5. Repeat steps no. 1 – 4 for each storage account available in the current Azure subscription.

6. Repeat steps no. 1 – 5 for each subscription created in your Microsoft Azure cloud account.










