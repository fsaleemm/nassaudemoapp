[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ffsaleemm%2Fsearchdemoapp%2Fmaster%2Ftemplates%2Fazuredeploy.json)

# Search Demo App

## Introduction

This is a demo application that is intended to be an illustration of how to index the output of the S2T data generated by the [solution accelerator](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/batch/batch-ingestion-client/Setup/guide.md). The search application will display top 100 search results and sample filters for demonstrating the search of the S2T transcription.

![Result Display](/images/result.JPG)

## Architecture

The following components will be deployed to your resource group.

![Components Deployed](/images/comp.PNG)

The diagram below shows the architecture.

![Architecture](/images/arch.PNG)

1. The blob container that has the output from the S2T solution accelerator.
1. The Azure Cognitive Search service is used to create an indexer to provide search capability for the JSON files create by S2T solution accelerator. 
1. A Web App service is used to deploy the custom search application.

## Setup Steps

1. Deploy the components using the Deploy to Azure button above.
1. Copy the [sample data](/searchqueryapp/SampleData) into the searchsource blob container. This container simulates the output storage container of the S2T solution accelerator. This is optional step if you have S2T  accelerator configured.
1. To create the indexer, use import data wizard instructions [here](https://docs.microsoft.com/en-us/azure/search/search-get-started-portal#create-index).
    1. In Step 1, for bullet 3, select Data Source as "Azure Blob Storage". Complete the wizard and select the storage account where the S2T files are located (searchsource blob container or output container for S2T solution).
    1. In Step 3, set index attributes as specified in the screenshot shown below. Make sure to select the Analyzers as "English - Microsoft":
    ![Index Definition](/images/indexdefinition.JPG)
1. Enable Authentication on the App Service. For simplicity [create new app registration automatically](https://docs.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad#-create-a-new-app-registration-automatically)
1. [Get the Search Service Endpoint and Key](https://docs.microsoft.com/en-us/azure/search/search-semi-structured-data#get-a-key-and-url), fill in the endpoint url and the api key. The index name is set in the indexer creation step above.

  ```json
  [
    {
      "name": "SearchIndexName",
      "value": "<Index Name>",
      "slotSetting": false
    },
    {
      "name": "SearchServiceEndpoint",
      "value": "<EndPoint URL>",
      "slotSetting": false
    },
    {
      "name": "SearchServiceQueryApiKey",
      "value": "<API Key>",
      "slotSetting": false
    }
  ]
  ```

6. Add the [configurations to the App Service.](https://docs.microsoft.com/en-us/azure/app-service/configure-common#edit-in-bulk).
1. Lauch the app.

Disclaimer: This is a sample application with rudimentary error/exception handling and no unit testing. It is intended as an illustration/demo and not for production use.
