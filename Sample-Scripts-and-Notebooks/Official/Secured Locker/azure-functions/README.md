# Using Azure Functions to decrypt and send retraining data to a Custom Vision project for retraining

This sample demonstrates how to create an Azure Function to decrypt the Azure-Percept-SMM retraining data and upload it to a Custom Vision project for model retraining. 

## Prerequisites

To run the sample, you need:

* [Install Visual Studio Code](https://code.visualstudio.com)
* [Config Visual Studio Code for Azure Functions development](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python)
* [Setup Custom Vision project and get project info](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/quickstarts/object-detection?tabs=visual-studio&pivots=programming-language-python)
* Service Principal (AZURE_CLIENT_ID, AZURE_CLIENT_SECRET and AZURE_TENANT_ID) which was recorded during Secure AI lifecycle service deployment.

## 1. Launch Visual Studio Code, create a new Azure Function project with the following settings:
* Language : Python
* Python interpreter: Python 3.6.x, 3.7.x 3.8.x are supported
* Template: Azure Blob Storage trigger
* Select a storage account: choose your Azure-Percept-SMM service storage account (i.e. testmmmodels)
* Blob storage path to be monitored: data

## 2. Add the following environment variables in local.settings.json with proper settings:
```
"AZURE_CLIENT_ID": "", 
"AZURE_CLIENT_SECRET": "",
"AZURE_TENANT_ID": "",
"mm_server_url": "",
"mm_storage_account": "",
"mm_telemtry_storage_container": "data",
"mm_model_name": "",
"mm_model_version": "",
"custom_vision_endpoint": "",
"custom_vision_training_key": "",
"custom_vision_project_id": ""
```
For example: 
```
"AZURE_CLIENT_ID": "33e5...",
"AZURE_CLIENT_SECRET": "c383...",
"AZURE_TENANT_ID": "72f9...",
"mm_server_url": "https://test-mm.westus2.cloudapp.azure.com",
"mm_storage_account": "testmmmodels",
"mm_telemtry_storage_container": "data",
"mm_model_name": "person-detection-retail",
"mm_model_version": "0013",
"custom_vision_endpoint": "https://cvdemo.cognitiveservices.azure.com/",
"custom_vision_training_key": "4240...",
"custom_vision_project_id": "2253..."
```
## 3. Grant the Service Principal as "Storage Blob Data Reader" in Azure-Percept-SMM storage account (defined as "mm_storage_account").   

## 4. Add the following dependencies in requirements.txt:
```
azure-identity
azure-storage-blob
azure-cognitiveservices-vision-customvision
sczpy-0.0.7-py3-none-any.whl
```

## 5. Copy [sczpy-0.0.7-py3-none-any.whl](../jupyter-basics) to your project folder. (TODO: changed the wheel installation to PyPI once the client SDK is published to PyPI.)

## 6. Update ```__init__.py``` with the following code:

```python
import logging
import os

from azure.storage.blob import BlobServiceClient, BlobClient, ContainerClient
from azure.identity import DefaultAzureCredential
from azure.cognitiveservices.vision.customvision.training import CustomVisionTrainingClient
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateBatch, ImageFileCreateEntry
from msrest.authentication import ApiKeyCredentials
import azure.functions as func

import sczpy

# Secure locker config
server_url = os.environ["mm_server_url"]
model_name = os.environ["mm_model_name"]
model_version = os.environ["mm_model_version"]
storage_name = os.environ["mm_storage_account"]
storage_container = os.environ["mm_telemtry_storage_container"]
storage_url = f"https://{storage_name}.blob.core.windows.net"

# Custom vision project config
custom_vision_endpoint = os.environ["custom_vision_endpoint"]
training_key = os.environ["custom_vision_training_key"]
project_id = os.environ["custom_vision_project_id"]

logging.info(f"Create data folder.")
data_dir = '/tmp'
if not os.path.exists(data_dir):
    os.makedirs(data_dir)

logging.info(f"Initialize sczpy client.")
client = sczpy.SCZClient(server_url)

logging.info(f"Initialize custom vision project.")
credentials = ApiKeyCredentials(in_headers={"Training-key": training_key})
trainer = CustomVisionTrainingClient(custom_vision_endpoint, credentials)
project = trainer.get_project(project_id)

def create_data_storage_client(blob_name):

    store_credential = DefaultAzureCredential()
    return BlobClient(storage_url, container_name=storage_container, blob_name=blob_name, credential=store_credential)

def main(myblob: func.InputStream):
    logging.info(f"Blob trigger function processed. Blob name: {myblob.name}")
    if model_name in myblob.name:
        # Download data file to local
        blob_name = myblob.name.replace('data/', '')
        download_file = os.path.join(data_dir,  blob_name)
        decrypted_file = os.path.join(data_dir, blob_name + '.dec.jpg')
        image_list = []

        storage_client = create_data_storage_client(blob_name)

        with open(download_file, "wb") as encypted_data:
            download_stream = storage_client.download_blob()
            encypted_data.write(download_stream.readall())
            # Decrpt data
            client.decrypt(model_name, model_version, download_file, decrypted_file)
        
        with open(decrypted_file, "rb") as image_contents:
            image_list.append(ImageFileCreateEntry(name=blob_name, contents=image_contents.read()))
        # Upload data to custom vision project
        upload_result = trainer.create_images_from_files(project.id, ImageFileCreateBatch(images=image_list))
        if not upload_result.is_batch_successful:
            logging.info(f"Image batch upload failed.")
            for image in upload_result.images:
                logging.info(f"Image status: {image.status}")
        else:
            logging.info(f"Image batch upload succeeded.")

        # Clean up temp files
        os.remove(download_file)
        os.remove(decrypted_file)   
```

## 7. Debug the project locally to make sure the retraining data can be fetched from the Azure-Percept-SMM storage blob, decrypted and uploaded to Custom Vision project as "Untagged" images.

## 8. Deploy the project to your Azure subscription and upload the local settings to cloud.