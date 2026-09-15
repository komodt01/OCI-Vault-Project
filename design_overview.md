# Design Overview – OCI Vault + Secrets Manager + IAM

## 1. Architecture Summary

This project uses OCI-native security services to establish centralized secret storage, encryption key management, and IAM-based access control.

The implemented architecture includes:

* **OCI Vault** for managing encryption keys
* **OCI Secrets Manager** for storing protected secret material
* **OCI IAM Policies** for controlling access to Vault and secret resources
* **Terraform** for defining and provisioning the supporting infrastructure

The goal is to separate secret storage from application configuration while establishing clear controls around encryption and authorization.

---

## 2. Core Architecture Flow

The implemented resource dependency is:

`OCI Authentication → Vault → Encryption Key → Secret → IAM Access`

The sequence matters.

1. Terraform authenticates to OCI using the configured API signing key.
2. The Vault is created.
3. An encryption key is created and associated with the Vault.
4. Secret material is stored through OCI Secrets Manager and protected using the Vault-managed key.
5. IAM policies determine which identities are authorized to manage or access the resources.

This dependency chain was an important part of the implementation because secret management depends on both cryptographic key management and identity authorization.

---

## 3. Security Controls Implemented

### Encryption at Rest

Secret material stored through OCI Secrets Manager is protected using the configured Vault-managed encryption key.

### IAM Authorization

OCI IAM policies provide access control for Vault, key, and secret resources.

### Centralized Secret Storage

The design avoids storing application credentials directly in source code or application configuration by providing a centralized secrets-management service.

### Infrastructure as Code

Terraform provides a repeatable definition of the OCI resources and their dependencies.

---

## 4. Production Architecture Extensions

A production implementation would require additional controls beyond the scope of this project.

These include:

* Automated secret rotation
* OCI Audit integration
* Centralized security monitoring and SIEM integration
* Alerting for suspicious secret or key activity
* Private service access where appropriate
* Protected remote Terraform state
* CI/CD secret scanning
* Log redaction
* Administrative approval and governance workflows

These controls were considered as part of the architecture but were not implemented or validated in this project.

---

## 5. Architectural Benefits

The design demonstrates several core security architecture principles:

* Separation of secret storage from application configuration
* Centralized encryption key management
* Least-privilege authorization
* Repeatable infrastructure provisioning
* Explicit resource dependencies
* Improved credential lifecycle governance
* A foundation for production monitoring and rotation controls

---

## 6. Deployment Model

Terraform was used to define and provision the OCI resources.

Terraform authentication required an RSA API signing key registered with OCI. The project also demonstrated practical considerations when administering OCI from WSL, including managing the authentication key material across the local environment.

For a production environment, Terraform state should also be protected using an appropriately secured remote backend with encryption, access restrictions, versioning, and lifecycle controls.
