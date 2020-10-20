# AI Model Protection at Rest

SCZ-SMM encrypts AI models before it saves them to disks. Each AI model version has its unique key, which is stored in Azure Key Vault. For a client to retrieve the secured key, it needs to authenticate with the SCZ-SMM server using a Service Principal that has been granted reading access to the Key Vault. 

The encrypted model files can be embedded into a Docker container, or be dynamically retrieved through SCZ-SMM Python SDK.

> **NOTE:** At the time of writing, SCZ-SMM doesn’t support customer-supplied keys.
