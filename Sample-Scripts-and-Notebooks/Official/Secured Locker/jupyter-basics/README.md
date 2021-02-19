# sczpy-basics.ipynb

This Notebook uses Azure-Percept-SMM Python SDK to preform a series of model operations:
> * Encryption (encrypts ```model.txt``` to ```model.txt.enc```)
> * Decryption (decrypts ```model.txt.enc``` to ``` model.decrypted.txt```)
> * Upload (uploads ```model.txt.enc```)
> * Download (downloads to ``` downloaded.txt.enc``` and then decryptes to ``` downloaded.decrypted.txt```)

1.	Before testing, you need to update the environment variables ```AZURE_CLIENT_ID```, ```AZURE_CLIENT_SECRET``` and ```AZURE_TENANT_ID``` to match with your service principal credential. Then, you need to update the ```server_url``` to point to your Azure-Percept-SMM server endpoint. For example:
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