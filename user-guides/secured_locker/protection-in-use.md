# AI Model Protection in Use

Protecting AI models in use from vogue administrators and hackers is a challenging problem. The goal of SCZ-SMM is to enable in-depth AI model protection when the model is in use. The evaluation SDK uses authentication and authorization to make sure AI models are only invoked by authorized service accounts. And you can mount the decrypted models in memory to provide certain protections from other processes on the host. At the time of writing, SCZ-SMM doesn’t offer loading models (as well as inference code) into trusted execution environments (TEE) but that’s something is being discussed for future versions. 

## Prevent unauthorized usages

Please see how SCZ-SMM uses [Authentication](TBD), [Authorization](TBD) and [Attestation](TBD) to avoid unauthorized usages. SMM also supports device attestation based on secrets stored in TPM to establish hardware root of trust, so that the AI models can only be decrypted and used by legit, untampered devices.

## Private AI Model algorithms

Algorithms such as [Differential Privacy](https://www.microsoft.com/en-us/ai/ai-lab-differential-privacy) and [Homomorphic Encryption](https://en.wikipedia.org/wiki/Homomorphic_encryption) can be used to provide in-depth privacy of AI models and training data.

> **NOTE:**  At the time of writing, SCZ-SMM doesn't offer specific integrations with these algorithms. Models that use these algorithms are managed by SCZ-SMM just like other model fiels.

## Sandboxed container runtimes

Sandboxed container runtimes such as [Kata](https://katacontainers.io/) and [gVisor](https://gvisor.dev/) offer strong isolations around container boundaries. 

> **NOTE:**  SCZ-SMM SDK works in OCI-compatible containers so it should work with above container runtimes but hasn’t been explicitly tested.

## Mount decrypted model to tmpfs

When running in container, SCZ-SMM writes the decrypted model to the writable layer, which is persisted on disk when the Container is stopped, and removed when the Container is removed.

You can choose to mount the decrypted model to Docker [tmpfs mounts](https://docs.docker.com/storage/tmpfs/). Tmpfs mounts are in memory so it's only accessible by the container process. It's removed when the container is stopped or removed. 

When condition permits (such as you have enough memory), using tmpfs elminiates the danger of clear-text models to be sniffed out of the file system.

> **NOTE:** When you use SCZ-SMM SDK in your code, you can use tempfs mounts as your targets of saving decrypted model files.

## Loading model to TEE

At the time of writing, SCZ-SMM doesn't have built-in integration with TEEs that allow models and inference logics to be loaded to protected enclaves to avoid information leak during execution. 


