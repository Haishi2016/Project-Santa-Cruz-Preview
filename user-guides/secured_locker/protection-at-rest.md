Azure Percept currently supports AI model protection as a preview feature. [Learn more](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/)

# AI model protection at rest

Azure Percept MM can encrypt AI models before saving them to the device. Each AI model version has its unique key, which is stored in Azure Key Vault. For a client to retrieve the secured key, it needs to authenticate with the Azure Percept MM service using a Service Principal that has been granted reading access to the Key Vault.

The encrypted model files can be embedded into a Docker container or dynamically retrieved through the Azure Percept MM SDK.

> **NOTE:** At the time of writing, Azure Percept MM doesn’t support customer-supplied keys.
