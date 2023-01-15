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
We will use ready github repository with predefined required functionality:
```
git clone https://github.com/MicrosoftDocs/mslearn-store-data-in-azure.git
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
```
To activate Azure storage (blobs) we will need to install folowing packages:
```
dotnet add package Azure.Storage.Blobs
dotnet restore
```
After sucessfull instaliation we will need to modify code as follow:<br>
Be sure that we are in project folder:
```
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
```
We can use comand "code . " (if we are in Azure enviroment) to update C# code:
```
code .
```
