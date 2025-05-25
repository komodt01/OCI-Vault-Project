# Technologies Used

## 1. OCI Vault
**What it is:** A managed key management service that allows secure storage and lifecycle management of encryption keys and secrets.
**Why we use it:** Centralized management of sensitive credentials.
**How it works:** It acts as a secure enclave for storing keys and secrets, protected via KMS.

## 2. OCI KMS Key
**What it is:** Oracle's Key Management Service provides encryption key generation and lifecycle control.
**Why we use it:** Required to encrypt secrets before storing in the Vault.
**How it works:** Used with AES-256 or other algorithms to encrypt content.

## 3. OCI Secrets Manager
**What it is:** Oracle's service to manage secrets like API tokens and DB passwords.
**Why we use it:** To enable secure, centralized secret access with encryption.
**How it works:** Uses a KMS key to encrypt BASE64-encoded content, stores in a vault.

## 4. Terraform
**What it is:** Infrastructure-as-Code tool for provisioning OCI resources.
**Why we use it:** Ensures repeatability and version-controlled deployments.
**How it works:** Declarative HCL syntax to define OCI infrastructure.

