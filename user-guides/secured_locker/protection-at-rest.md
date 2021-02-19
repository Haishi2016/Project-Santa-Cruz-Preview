# AI model protection at rest

Azure-Percept-SMM encrypts AI models before it saves them to disk. Each AI model version has its unique key, which is stored in Azure Key Vault. For a client to retrieve the secured key, it needs to authenticate with the Azure-Percept-SMM server using a Service Principal that has been granted reading access to the Key Vault.

The encrypted model files can be embedded into a Docker container or dynamically retrieved through the Azure-Percept-SMM Python SDK.

> **NOTE:** At the time of writing, Azure-Percept-SMM doesn’t support customer-supplied keys.
