# Using SCZ-SMM Python SDK in your Python program

This sample program uses SCZ-SMM Python SDK to preform a series of model operations:
> * Encryption (encrypts ```model.txt``` to ```model.txt.enc```)
> * Decryption (decrypts ```model.txt.enc``` to ``` model.decrypted.txt```)
> * Upload (uploads ```model.txt.enc```)
> * Download (downloads to ``` downloaded.txt.enc`` and then decryptes to ``` downloaded.decrypted.txt```)

1.	Before testing, you need to update the environment variables ```AZURE_CLIENT_ID```, ```AZURE_CLIENT_SECRET``` and ```AZURE_TENANT_ID``` to match with your service principal credential. Then, you need to update the ```server_url``` to point to your Santa Cruz server endpoint. For example:
    ```python
    os.environ["AZURE_CLIENT_ID"] = "555d..."
    os.environ["AZURE_CLIENT_SECRET"] = "6da3..."
    os.environ["AZURE_TENANT_ID"] = "72f9..."
    server_url = "https://scz-mm1.westus2.cloudapp.azure.com"
    ```
2.	Run the program

    ```bash
    python app.py
    ```
3.	Observe a few files get created under the application folder:
    ```bash
    model.txt.enc
    model.decrypted.txt
    downloaded.txt.enc
    downloaded.decrypted.txt
    ```
> **NOTE:** You'll see a few certificate warnings. These are caused by the self-signed certificate used by SCZ-SMM server deployment. You can ignore these warnings, or replace the certificate with a trusted certificate.

```bash
InsecureRequestWarning: Unverified HTTPS request is being made to host 'scz-mm1.westus2.cloudapp.azure.com'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
```