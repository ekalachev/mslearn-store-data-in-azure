 - Create resource group
 
    - az group create --name "file-uploader-dev-rg" --location "West Europe" 
    

 - Create azure storage account
 
   If you use Azure-CLI:
   
      - az storage account create \
        --kind StorageV2 \
        --resource-group [your-resourse-group] \
        --location centralus \
        --name [your-unique-storage-account-name]

- Deploy and run in Azure

  If you use Azure-CLI:
  
    - Create an App Service app and configure it with application settings for our storage account connection string and container name. Get the storage account's connection string with az storage account show-connection-string and set the name of the container to be files. The app name needs to be globally unique, so you'll need to choose your own name to fill in <your-unique-app-name>. Run commands:
    
        - az appservice plan create --name blob-exercise-plan --resource-group [your-resourse-group] --sku FREE --location centralus
        
        - az webapp create --name [your-unique-app-name] --plan blob-exercise-plan --resource-group [your-resourse-group]
        
        - $CONNECTIONSTRING=$(az storage account show-connection-string --name [your-unique-storage-account-name] --output tsv)
        
        - az webapp config appsettings set --name [your-unique-app-name] --resource-group [your-resourse-group] --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files
    
    - Deploy your project:
    
      Make sure your shell is still in the [your-project-dirrectory] directory before running the following commands. Run commands:
      
        - dotnet publish -o pub        
        
        - cd pub
        
        - zip -r ../site.zip *
        
        - az webapp deployment source config-zip --src ../site.zip --name [your-unique-app-name] --resource-group [your-resourse-group]
        
      ![Result](https://docs.microsoft.com/en-us/learn/modules/store-app-data-with-azure-blob-storage/media/7-fileuploader-empty.png)
