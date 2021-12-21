## Simple setup on how to deploy Custom VMs to Azure via packer


Link to the main [Azure Packer docs](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/build-image-with-packer)


To get started from scratch: 

- get the azure CLI tools 
- login to your Azure account: `az login`
- create a resourceGroup: `az group create -n myResourceGroupName -l westeurope` 
- create azure credentials (packer authenticates to azure via a service principal): `az ad sp create-for-rbac --role Contributor --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
`
- At this stage you should get the output: 
```
{
    "client_id": "xxxx-xxxx-xxxx-xxxx-xxxx",
    "client_secret": "xxxx-xxxx-xxxx-xxxx-xxxx",
    "tenant_id": "xxxx-xxxx-xxxx-xxxx-xxxx"
}
```
- To authenticate to Azure, you need to get the subscription ID of the resource group that you want packer to target: 
`az account show --query "{ subscription_id: id }"`
- At this stage after filling out the ubuntu.json packer template with your own values, you should be able to run the `ubuntu.json` packer definition in the `infra` directory: 

``` 
packer build infra/ubuntu.json
```
* This initial build tends to take around 6 - 7 minutes