# Azure Ubuntu 18.04 VM with a system assigned Managed Identity and BlubFuse installed.

## Prerequisite 
```
git clone https://github.com/derdanu/azure-ubuntu-blobfuse.git
cd azure-ubuntu-blobfuse
```
## Create a single Azure VM using CLI and the cloudinit file 
```
export GROUP=blobfuse
az group create --name $GROUP --location westeurope
az vm create \
    --resource-group $GROUP \
    --name blobfuse --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Test123#123# \
    --nsg-rule "SSH"  \
    --public-ip-sku Standard \
    --assign-identity \
    --custom-data cloud-init.txt
```
## Mount an Storage Account with the Identity as Root user
Please keep in mind you need to grant the Identity permissions on your Storage Account first. 
```
export AZURE_STORAGE_ACCOUNT=dafalknemadphfs
export AZURE_STORAGE_AUTH_TYPE=MSI
mkdir -p /mnt/storage
mkdir -p /mnt/blobfusetmp
blobfuse /mnt/storage --container-name=test --tmp-path=/mnt/blobfusetmp
```
## Clean up
```
az group delete -n $GROUP
```