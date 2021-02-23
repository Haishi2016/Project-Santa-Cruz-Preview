# AI Model Protection in transit

Azure-Percept-SMM transfers AI models and secret keys over the TLS (Transport Layer Security) channel once the client has successfully authenticated.

When you deploy the Azure-Percept-SMM environment using the deployment script, the script generates a self-signed certificate to be associated with the Azure Application Gateway instance that serves as the front door of the Azure-Percept-SMM web server. Azure Application Gateway does TLS termination and talks to the Azure-Percept-SMM server over a private network in plain text (HTTP).

It's also possible to configure end-to-end SSL, as introduced [here](https://docs.microsoft.com/en-us/azure/application-gateway/end-to-end-ssl-portal).

> **NOTE:** In future versions, the Azure-Percept-SMM will use a device-specific certificate to provide additional protection; when the device requests encrypted models or secret keys, it will send its corresponding public key to Azure-Percept-SMM, and Azure-Percept-SMM will use the public key to encrypt the payload before it sends it back to the client over TLS.
