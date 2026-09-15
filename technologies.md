# Technologies – OCI Vault + Secrets Manager + IAM

This document describes the primary technologies used in the OCI Vault project and distinguishes between components implemented directly and capabilities considered for a production architecture.

---

## OCI Vault and Key Management

**What it is:**
OCI-native capability for managing cryptographic keys used to protect cloud resources and sensitive information.

**Why I used it:**
The project required a centralized encryption mechanism for protecting secret material rather than relying on application-managed encryption.

**How it was used:**
A Vault and encryption key were provisioned as part of the Terraform deployment. The encryption key provides the cryptographic protection used by the secrets-management architecture.

This established the dependency:

`Vault → Encryption Key → Secret`

**Project status:** **Implemented**

---

## OCI Secrets Management

**What it is:**
OCI-managed capability for storing sensitive values such as passwords, API credentials, tokens, and other application secrets.

**Why I used it:**
Centralized secret storage reduces the need to place sensitive credentials directly in source code, application configuration, or deployment artifacts.

**How it was used:**
Secret resources were defined as part of the Terraform implementation and protected using the Vault-managed encryption key.

Runtime application retrieval and automated rotation were considered as production architecture extensions but were not implemented in this project.

**Project status:** **Implemented for secret storage**

---

## OCI IAM

**What it is:**
OCI's identity and authorization system for controlling access to cloud resources.

**Why I used it:**
Encryption alone does not determine who should be allowed to interact with a secret. IAM provides the authorization layer around Vault, key, and secret operations.

**How it was used:**
IAM policies were incorporated into the project to control access to the OCI resources.

For a production workload, I would extend the identity model with appropriately scoped workload identities, such as OCI dynamic groups and resource or instance principals where applicable, rather than distributing long-lived user credentials to applications.

**Project status:** **Implemented for IAM policy controls**

---

## Terraform

**What it is:**
Infrastructure as Code tooling used to define and manage cloud resources.

**Why I used it:**
Terraform provided a repeatable way to provision the Vault, encryption key, secret, and IAM resources while making their dependencies explicit.

**How it was used:**
The project used the OCI Terraform provider and Terraform configuration files to provision the environment.

Terraform authentication to OCI required RSA API signing key configuration.

Terraform also exposed several security considerations that would need to be addressed in production, including:

* Provisioning identity permissions
* Terraform state protection
* Change governance
* Provider version management
* Resource lifecycle management

**Project status:** **Implemented**

---

## RSA API Signing Key Authentication

**What it is:**
An OCI authentication mechanism that allows API and tooling access using a registered public/private key pair.

**Why I used it:**
Terraform required an authenticated identity before it could provision OCI resources.

**How it was used:**
An RSA key pair was configured and the public key registered with the OCI user profile so Terraform could authenticate to OCI.

Working from WSL also introduced a practical cross-platform consideration when managing and registering the authentication material.

**Project status:** **Implemented**

---

## OCI Audit

**What it is:**
OCI capability for recording API activity and providing visibility into administrative operations.

**Why it matters:**
A production secrets-management architecture needs evidence of security-relevant activity involving keys, secrets, IAM policies, and administrative changes.

**How it fits the architecture:**
OCI Audit would form part of the production monitoring and investigation architecture and could feed centralized monitoring or SIEM capabilities.

Audit integration and alerting were not implemented or validated in this project.

**Project status:** **Production architecture extension**

---

## Workload Identity

OCI supports identity patterns that allow cloud workloads to interact with authorized resources without embedding long-lived OCI user credentials in applications.

For a production implementation, workload identities could be authorized to retrieve only the secrets required by a specific application.

The intended pattern would be:

`Application → Workload Identity → IAM Authorization → Secret`

This capability was considered as part of the architecture but was not implemented or tested in this project.

**Project status:** **Production architecture extension**

---

## Secret Rotation

Credential rotation limits the exposure period associated with long-lived secrets.

A production implementation should establish rotation requirements based on credential type, application dependencies, operational impact, and organizational policy.

Automated secret rotation was not implemented in this project.

**Project status:** **Production architecture extension**

---

## Technology Architecture

The core technology chain implemented by the project was:

`Administrator → OCI Authentication → Terraform → Vault → Encryption Key → Secret → IAM Access`

A more complete production architecture would extend that foundation with:

`Workload Identity → Runtime Retrieval → Rotation → Audit → Monitoring/SIEM → Lifecycle Governance`

The distinction is important: the project implemented the core secrets-management foundation while the additional capabilities represent how I would evolve that foundation for production use.

---

## Summary

The project provided hands-on experience with OCI Vault, encryption key management, Secrets Management, IAM, Terraform, and OCI API authentication.

It also demonstrated that the technology used to store a secret is only one part of the security architecture. Identity, authorization, encryption, infrastructure provisioning, monitoring, rotation, and lifecycle governance all contribute to the security of the overall solution.
