# Securing your AI model and data

Santa Cruz provides AI model protections [at rest](protection-at-rest.md), [during transition](protection-in-transition.md) and [in use](protection-in-use.md). It's designed to work with existing AI systems and workflows such as [Azure Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/), [Azure Databricks](https://azure.microsoft.com/en-us/services/databricks/), and [Azure Cognitive Services](https://azure.microsoft.com/en-us/services/cognitive-services/). The long-term goal of Santa Cruz is to provide a unified experience across underlying systems.

The Santa Cruz preview kit is shipped with a secured AI model locker and a Python SDK, as highlighted in the following diagram. The [server](server-topology.md) provides secured key and model management capabilities. In the future, the SDK will interact with the device TPM and attestation service to prove device identity with the server to retrieve protected keys or models.

![Architecture](./imgs/architecture.png)

To get started with provisioning a secured locker, see this [quickstart](provision-a-secured-locker.md).