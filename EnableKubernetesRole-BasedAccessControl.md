

# Enable Kubernetes Role-Based Access Control



## Audit

#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to **All resources** blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. Choose the Azure subscription that you want to access from the **Subscription** filter box.

4. From the **Type** filter box, select **Kubernetes service** to list only the Azure Kubernetes Service (AKS) clusters available in the selected subscription.

5. Click on the name (link) of the AKS cluster that you want to examine.

6. In the navigation panel, under **Settings**, select **Properties** to view the selected AKS resource properties.

7. On the **Properties** panel, check the value assigned to the **RBAC** configuration attribute. If this value is set to **Disabled**, the Kubernetes Role-Based Access Control (RBAC) is not enabled for the selected Azure Kubernetes Service (AKS) cluster.

8. Repeat steps no. 5 – 7 for each AKS cluster provisioned in the selected Azure subscription.

9. Repeat steps no. 3 – 8 for each subscription created in your Microsoft Azure cloud account.



#### Using Azure CLI

1. Run **aks list** command (Windows/macOS/Linux) using custom query filters to list the name and the associated resource group for each Azure Kubernetes Service (AKS) cluster available in the current subscription:

```
az aks list
	--output table
	--query '[*].{name:name, resourceGroup:resourceGroup}'
```



2. The command output should return the requested AKS cluster names:

```
Name                    ResourceGroup
----------------------  ------------------------------
cc-data-mining-cluster  cloud-shell-storage-westeurope
cc-project5-cluster     cloud-shell-storage-westeurope
```



3. Run **aks show** command (Windows/macOS/Linux) using the name of the AKS cluster that you want to examine and its associated resource group as identifier parameters to describe the configuration status of the Kubernetes Role-Based Access Control function set for the selected AKS resource:

```
az aks show
	--name cc-data-mining-cluster
	--resource-group cloud-shell-storage-westeurope
	--query 'enableRbac'
```



4. The command output should return the requested function configuration status (**true** for enabled, **false** for disabled)

```
false
```

>  If the command output returns **false**, as shown in the example above, the Kubernetes Role-Based Access Control (RBAC) is not enabled for the selected Azure Kubernetes Service (AKS) cluster.

5. Repeat step no. 3 and 4 for each AKS cluster available within the current Azure subscription.

6. Repeat steps no. 1 – 5 for each subscription created in your Microsoft Azure cloud account.



## Remediation

#### Using Azure Console

1. Sign in to Azure Management Console.

2. Navigate to All resources blade at https://portal.azure.com/#blade/HubsExtension/BrowseAll to access all your Microsoft Azure resources.

3. Choose the Azure subscription that you want to access from the **Subscription** filter box.

4. From the **Type** filter box, select **Kubernetes service** to list only the Azure Kubernetes Service (AKS) clusters available in the selected subscription.

5. Click on the name of the AKS cluster that you want to re-create and copy the necessary configuration details such as associated resource group, Azure region, Kubernetes version, node size and count, scaling and networking settings and so on. This configuration information will be required later when the new AKS cluster will be launched.

6. Navigate to Azure Kubernetes Service blade at https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerService%2FmanagedClusters and click +Add to initiate the cluster setup wizard.

7. On the **Create Kubernetes cluster** page, configure the AKS new cluster using the configuration information copied at step no. 5:
   1. On the **Basics** panel, provide a name for the new cluster, select the resource group where the resource will be created, select the appropriate Azure region and Kubernetes version, and configure the cluster node size and count. Click **Next : Scale** to continue the setup process.
   2. On the **Scale** panel, configure the scaling/autoscaling settings required for the new cluster, then click **Next : Authentication** to continue.
   3. On the **Authentication** panel, configure the service principal, then click **Yes** next to **Enable RBAC** to enable Kubernetes Role-Based Access Control (RBAC) for the cluster. This will implement granular access to Kubernetes resources that support RBAC controls within the new AKS cluster. Click **Next : Networking** to continue the resource setup process.
   4. On the **Networking** panel, configure the networking settings for the new AKS cluster as seen at step. no 5, then click **Next : Monitoring** to continue.
   5. On the **Monitoring** panel, configure container monitoring, then click **Next : Tags** to continue the process.
   6. On the **Tags** panel, create the necessary tags and click **Next : Review + Create** to validate the configuration information provided during setup.
   7. On the **Review + Create** panel, review the configuration details, then click **Create** to launch the new Azure Kubernetes Service (AKS) cluster. The provisioning process should take a few minutes to complete.

8. Repeat steps no. 5 – 7 for each Azure Kubernetes Service cluster that you want to relaunch in order to enable Kubernetes Role-Based Access Control (RBAC), available in the selected subscription.

9. Repeat steps no. 3 – 8 for each subscription created in your Microsoft Azure cloud account.





#### Using Azure CLI

1. Run **aks show** command (OSX/Linux/UNIX) using the name of the cluster that you want to re-create as identifier parameter (see Audit section part II to identify the right resource) and custom query filters to get the specified AKS cluster configuration details. The information requested will be required later when the new Azure Kubernetes Service cluster will be launched:

```
az aks show
	--name cc-data-mining-cluster
	--resource-group cloud-shell-storage-westeurope
```



2. The command output should return the requested function configuration information:

```
{
  "aadProfile": null,
  "addonProfiles": {
    "httpApplicationRouting": {
      "config": null,
      "enabled": false
    }
  },
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "count": 3,
      "enableAutoScaling": null,
      "maxCount": null,
      "maxPods": 110,
      "minCount": null,
      "name": "agentpool",
      "orchestratorVersion": "1.13.10",
      "osDiskSizeGb": 150,
      "osType": "Linux",
      "provisioningState": "Succeeded",
      "type": "AvailabilitySet",
      "vmSize": "Standard_B2s"
    }
  ],
  "apiServerAuthorizedIpRanges": null,
  "dnsPrefix": "cc-data-mining-cluster-dns",
  "enablePodSecurityPolicy": null,
  "enableRbac": false,
  "fqdn": "cc-data-mining-cluster-dns-abcdabcd.hcp.westeurope.azmk8s.io",
  "id": "/subscriptions/abcdabcd-1243-abcd-1243-abcd1243abcd/resourcegroups/cloud-shell-storage-westeurope/providers/Microsoft.ContainerService/managedClusters/cc-data-mining-cluster",
  "identity": null,
  "kubernetesVersion": "1.13.10",
  "linuxProfile": null,
  "location": "westeurope",
  "maxAgentPools": 2,
  "name": "cc-data-mining-cluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.18.0.1/16",
    "loadBalancerSku": "Basic",
    "networkPlugin": "kubenet",
    "networkPolicy": null,
    "podCidr": "10.182.0.0/16",
    "serviceCidr": "10.0.0.0/16"
  },
  "nodeResourceGroup": "MC_cloud-shell-storage-westeurope_cc-data-mining-cluster_westeurope",
  "resourceGroup": "cloud-shell-storage-westeurope",
  "servicePrincipalProfile": {
    "clientId": "1234abcd-1243-abcd-1243-abcd1243abcd"
  },
  "type": "Microsoft.ContainerService/ManagedClusters",
  "windowsProfile": null
}
```



3. Run **ad sp create-for-rbac** command (OSX/Linux/UNIX) to generate the service principal required for AKS cluster creation. Use the **--skip-assignment** parameter to prevent any additional default assignments from being allocated:

```
az ad sp create-for-rbac
	--skip-assignment
```



4. The command output should return the configuration metadata for the newly created service principal:

```
{
  "appId": "01234567-1234-1234-1234-123456789012",
  "displayName": "azure-cli-2019-09-20-12-02-11",
  "name": "http://azure-cli-2019-09-20-12-02-11",
  "password": "abcdabcd-1234-abcd-1234-abcd1234abcd",
  "tenant": "aaaabbbb-aaaa-bbbb-cccc-aaaabbbbcccc"
}
```



4. Run **aks create** command (OSX/Linux/UNIX) to launch a new Azure Kubernetes Service (AKS) cluster with the Kubernetes Role-Based Access Control (RBAC) function enabled, using the cluster configuration details returned at step no. 2 and 4. For **--service-principal** command parameter use the **"appId"** attribute value returned at step no. 4 and for **--client-secret** parameter use the **"password"** attribute value (the command does not produce an output):

```
az aks create
	--name cc-data-mining-rbac-cluster
	--resource-group cloud-shell-storage-westeurope
	--location "westeurope"
	--node-count 3
	--node-vm-size "Standard_B2s"
	--node-osdisk-size 150
	--kubernetes-version "1.13.10"
	--service-principal "01234567-1234-1234-1234-123456789012"
	--client-secret "abcdabcd-1234-abcd-1234-abcd1234abcd"
	--no-ssh-key
	--enable-rbac 
```



6. Repeat steps no. 1 – 5 for each Azure Kubernetes Service cluster that you want to re-create in order to enable Kubernetes Role-Based Access Control (RBAC), available within the current subscription.

7. Repeat steps no. 1 – 6 for each subscription created in your Microsoft Azure cloud account.



