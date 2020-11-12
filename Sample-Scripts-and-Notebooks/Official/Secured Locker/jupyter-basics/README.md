# sczpy-basics.ipynb

This Notebook uses SCZ-SMM Python SDK to preform a series of model operations:
> * Encryption (encrypts ```model.txt``` to ```model.txt.enc```)
> * Decryption (decrypts ```model.txt.enc``` to ``` model.decrypted.txt```)
> * Upload (uploads ```model.txt.enc```)
> * Download (downloads to ``` downloaded.txt.enc`` and then decryptes to ``` downloaded.decrypted.txt```)

1.	Before testing, you need to update the environment variables ```AZURE_CLIENT_ID```, ```AZURE_CLIENT_SECRET``` and ```AZURE_TENANT_ID``` to match with your service principal credential. Then, you need to update the ```server_url``` to point to your Santa Cruz server endpoint. For example:
    ```
    %env AZURE_CLIENT_ID="555d..."
    %env AZURE_CLIENT_SECRET="6da3..."
    %env AZURE_TENANT_ID="72f9..."
    server_url = "https://scz-mm1.westus2.cloudapp.azure.com"
    ```
2.	Run all cells in the Notebook.

3.	Observe a few files get created under the application folder:
    ```bash
    model.txt.enc
    model.decrypted.txt
    downloaded.txt.enc
    downloaded.decrypted.txt
    ```

# azure-eye-container-deployment.ipynb

This Notebook deploys Azure Eye container to Santa Cruz Devkit devices for secure locker testing:

1.	Before deployment, you need to update the parameters for container deployment. For example:
    ```
    subscription_id="9934..."
    iot_hub_name="MyIotHub"
    iot_device_id="devkit..."
    azure_client_id="555d..."
    azure_client_secret="6da3..."
    azure_tenent_id="72f9..."
    scz_model_name="facedetection"
    scz_model_version="v1"
    scz_mm_server_url="https://scz-mm1.westus2.cloudapp.azure.com"
    confidence_threshold="80"
    ```

2.	Run all cells in the Notebook.

3.  The Notebook pops up a web browser for you to log in to your Azure subscription. Login to your Azure account to continue.

4.  You will see the Azure Eye container deployment result after the execution completes, and you will see the container named "azureeyemodule" running on the Santa Cruz DevKit.
