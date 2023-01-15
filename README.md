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
<img src="https://github.com/WojtekSza/File-Storage-Service/blob/main/File%20Upload/1.jpg" width="800"/>  <br>
Locate 'Models/BlobStorage.cs' file and modify code as follow:<br>
Add Azure storage libraries:
```
using Azure;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```
Next replace current implementation of Initialize method to following and SAVE (CTRL+S):
```
public Task Initialize()
{
    BlobServiceClient blobServiceClient = new BlobServiceClient(storageConfig.ConnectionString);
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(storageConfig.FileContainerName);
    return containerClient.CreateIfNotExistsAsync();
}
```
<img src="https://github.com/WojtekSza/File-Storage-Service/blob/main/File%20Upload/2.jpg" width="800"/>  <br>
Locate 'Models/BlobStorage.cs' file and modify code as follow:<br>
To show all storage containers in Azure we will replace GetNames method in BlobStorage.cs as follows and then SAVE (CTRL+S):
```
public async Task<IEnumerable<string>> GetNames()
{
    List<string> names = new List<string>();

    BlobServiceClient blobServiceClient = new BlobServiceClient(storageConfig.ConnectionString);

    // Get the container the blobs are saved in
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(storageConfig.FileContainerName);

    // This gets the info about the blobs in the container
    AsyncPageable<BlobItem> blobs = containerClient.GetBlobsAsync();

    await foreach (var blob in blobs)
    {
        names.Add(blob.Name);
    }
    return names;
}
```
To set UPLOAD functions we will need modify Save methoed in in BlobStorage.cs as follows:<br>
```
public Task Save(Stream fileStream, string name)
{
    BlobServiceClient blobServiceClient = new BlobServiceClient(storageConfig.ConnectionString);

    // Get the container (folder) the file will be saved in
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(storageConfig.FileContainerName);

    // Get the Blob Client used to interact with (including create) the blob
    BlobClient blobClient = containerClient.GetBlobClient(name);

    // Upload the blob
    return blobClient.UploadAsync(fileStream);
}
```
