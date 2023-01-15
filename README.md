# File-Storage-Service
Repository of file website service (download, upload, delete) written in C# and launched in Azure Cloud.<br>
First step is to logi in into azure service via CLI:
```
az login
```
After sucesfull authentication next step is to create Azure resource group:
```
az group create -l francecentral -n FileService
```
Next step is to create storage account:
```
az storage account create --kind StorageV2 --resource-group FileService --location francecentral --name fileserviceaccount
```
