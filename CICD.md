- create variables:
  - $group = "file-uploader-dev-rg"
  - $location = "West Europe"
  - $storageAccount = "fileuploaderstorage2020"
  - $appServicePlan = "myNameOfAPlan"
  - $webAppName = "file-uploader"

- create resource group
  - az group create --name $group --location $location

- create storage account
  - az storage account create --kind StorageV2 --resource-group $group --location $location --name $storageAccount

- create service plan
  - az appservice plan create --name $appServicePlan --resource-group $group --sku FREE --location $location

- create web apps as many as you need
  - az webapp create --name ("dev-" + $webAppName) --plan $appServicePlan --resource-group $group
  - az webapp create --name ("qa-" + $webAppName) --plan $appServicePlan --resource-group $group
  - az webapp create --name ("production-" + $webAppName) --plan $appServicePlan --resource-group $group

- web app additional settings
  - $CONNECTIONSTRING=$(az storage account show-connection-string --name $storageAccount --output tsv)
  - az webapp config appsettings set --name ("dev-" + $webAppName) --resource-group $group --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files
  - az webapp config appsettings set --name ("qa-" + $webAppName) --resource-group $group --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files
  - az webapp config appsettings set --name ("production-" + $webAppName) --resource-group $group --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files

### after these steps, you can setup your environment on https://dev.azure.com/


