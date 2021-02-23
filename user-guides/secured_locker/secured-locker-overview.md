# Securing your AI model and data

Azure Percept provides AI model protections [at rest](protection-at-rest.md) and [in transit](protection-in-transit.md). It's designed to work with existing AI systems and workflows such as [Azure Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/), [Azure Databricks](https://azure.microsoft.com/en-us/services/databricks/), and [Azure Cognitive Services](https://azure.microsoft.com/en-us/services/cognitive-services/). The long-term goal of Azure Percept is to provide a unified experience across underlying systems.

The Azure Percept Studio (Azure-Percept-SMM) can be configured with an AI model locker and a Python SDK, as highlighted in the following diagram. The [locker](server-topology.md) provides key and model management capabilities. In the future, the SDK will interact with the device TPM and attestation service to prove device identity with the server to retrieve protected keys or models.

![Architecture](./imgs/architecture.png) 

To get started with provisioning a secured locker, see this [quickstart](provision-a-secured-locker.md).
