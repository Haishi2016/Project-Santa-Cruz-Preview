# AI model protection in use

Protecting AI models in use from rogue administrators and hackers is a challenging problem. The goal of SCZ-SMM is to enable in-depth AI model protection when the model is in use. The evaluation SDK uses authentication and authorization to make sure AI models are only invoked by authorized service accounts. You can also mount the decrypted models in memory to provide certain protections from other processes on the host. At the time of writing, SCZ-SMM doesn’t offer the capability to load models or inference code into trusted execution environments (TEE), but this feature may become available in a future version.

## Prevent unauthorized usages

SCZ-SMM uses [Authentication](TBD), [Authorization](TBD), and [Attestation](TBD) to avoid unauthorized usages. SCZ-SMM also supports device attestation based on secrets stored in the TPM to establish hardware root of trust, which limits AI model decryption and use to legitimate, untampered devices. The attestation features will be updated in a future version of the SantaCruz Secure AI Lifecycle SDK.

## Private AI Model algorithms

Algorithms such as [differential privacy](https://www.microsoft.com/en-us/ai/ai-lab-differential-privacy) and [homomorphic encryption](https://en.wikipedia.org/wiki/Homomorphic_encryption) can be used to provide in-depth privacy of AI models and training data.

> **NOTE:**  At the time of writing, SCZ-SMM doesn't offer specific integrations with these algorithms. Models that use these algorithms are managed by SCZ-SMM just like other model fields.

## Sandboxed container runtimes

Sandboxed container runtimes such as [Kata](https://katacontainers.io/) and [gVisor](https://gvisor.dev/) offer strong isolations around container boundaries.

> **NOTE:**  The SCZ-SMM SDK works in OCI-compatible containers, so it should work with the above container runtimes, but they haven’t been explicitly tested.

## Mount decrypted model to tmpfs

When running in a container, SCZ-SMM writes the decrypted model to the writable layer, which is persisted on disk when the container is stopped and removed when the container is removed.

You can choose to mount the decrypted model to [Docker tmpfs mounts](https://docs.docker.com/storage/tmpfs/). Tmpfs mounts are in memory, so the model is only accessible by the container process. The model is removed when the container is stopped or removed.

When condition permits (e.g. when you have enough memory), using tmpfs eliminates the danger of clear-text models being sniffed out of the file system.

> **NOTE:** When you use the SCZ-SMM SDK in your code, you can use tmpfs mounts as your targets for saving decrypted model files.

## Loading model to TEE

At the time of writing, SCZ-SMM doesn't have built-in integration with TEEs that allow models and inference logics to be loaded to protected enclaves to avoid information leak during execution.