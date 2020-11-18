# AI Model Protection in Transit

SCZ-SMM transfers AI models and secret keys over TLS channel, once the client has successfully authenticated. 

At the time of writing, when you deploy SCZ-SMM environment using the deployment script, the script generates a self-signed certificate to be associated with the Azure Application Gateway instance that serves as the front door of the SCZ-SMM web server. Azure Application Gateway does TLS termination and talks to the SCZ-SMM server over a private network in plain text (HTTP).

It's also possible to configure end-to-end SSL, as introduced [here](https://docs.microsoft.com/en-us/azure/application-gateway/end-to-end-ssl-portal).

> **NOTE:** In future versions, the SCZ-SMM will use a device-specific certificate to provide additional protection: When the device requests encrypted models or secret keys, it sends it's corresponding public key to SCZ-SMM, and SCZ-SMM uses the public key to encrypt the payload before it sends it back to the client over TLS. 
