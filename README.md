# OCI Vault, Secrets Management & IAM Security Architecture

## Project Overview

This project demonstrates how sensitive application credentials can be protected in Oracle Cloud Infrastructure (OCI) using OCI Vault, encryption keys, Secrets Manager, IAM policies, and Terraform.

The project focuses on a common security problem: applications need access to passwords, API keys, tokens, and other credentials, but those secrets should not be embedded in source code, configuration files, or application images.

For this scenario, I used OCI-native security services to separate secret storage, encryption key management, and access authorization while managing the supporting resources through Infrastructure as Code.

---

## Business Problem

Application credentials create risk when they are stored directly in code, configuration files, deployment artifacts, or other locations where access may be difficult to control and audit.

The architecture needed to address several questions:

* Where should sensitive credentials be stored?
* How should those credentials be encrypted?
* Who should be allowed to access or manage them?
* How can infrastructure configuration be made repeatable?
* What additional controls would be required for a production environment?

The goal was to establish a centralized secrets-management pattern using OCI-native capabilities rather than distributing credentials throughout application environments.

---

## Project Scope

### Implemented

The project implemented and worked through:

* OCI Vault
* Vault-managed encryption key
* OCI Secrets Manager
* OCI IAM policies
* Terraform-based resource provisioning
* OCI provider configuration
* RSA key-based Terraform authentication to OCI
* Vault, key, and secret resource dependencies
* Secret encryption at rest
* IAM-based access control
* Resource teardown and lifecycle considerations

### Production Controls Considered

Several controls were evaluated as part of the architecture but were not implemented in this project:

* Automated secret rotation
* OCI Audit integration and monitoring
* SIEM integration and alerting
* Private service access/endpoints
* CI/CD secret scanning
* Production governance and approval workflows
* Hardened remote Terraform state management
* Automated log redaction and monitoring controls

These are treated as production architecture considerations rather than completed implementation.

---

## Architecture Flow

The core security flow is:

1. **Create the OCI Vault**

   * Establish the managed location for cryptographic keys.

2. **Create the Encryption Key**

   * Associate the encryption key with the Vault.
   * Use the key to protect secret material stored through OCI Secrets Manager.

3. **Create and Store Secrets**

   * Store application credentials centrally rather than embedding them in application configuration or source code.
   * Protect stored secret material using the Vault-managed key.

4. **Apply IAM Policies**

   * Define which identities are allowed to manage or access Vault and secret resources.
   * Limit access according to least-privilege principles.

5. **Application Secret Retrieval**

   * In a production implementation, workloads can retrieve authorized secrets at runtime rather than storing long-lived credentials locally.

6. **Monitoring and Lifecycle Governance**

   * Production environments should extend the design with auditing, monitoring, rotation, alerting, and lifecycle controls.

---

## Infrastructure as Code

Terraform was used to define and provision the OCI resources.

One implementation lesson was that the resources have dependencies that must be understood rather than treated as independent components.

The basic dependency chain is:

`OCI Authentication → Vault → Encryption Key → Secret → IAM Access`

Terraform also required OCI authentication using an RSA key pair registered with the OCI user profile.

Working from WSL introduced an additional setup consideration because the public key used by Terraform needed to be registered with OCI correctly across the Windows/Linux environment boundary.

This reinforced an important architecture point: the security of the secrets platform begins with the identity and authentication mechanism used to provision it.

---

## Security Controls

### Encryption at Rest

Secret material is protected using OCI Vault-managed encryption keys.

### IAM Authorization

OCI IAM policies control access to Vault, key, and secret resources and provide the foundation for least-privilege administration.

### Centralized Secret Storage

Credentials are stored through OCI Secrets Manager rather than distributed through application configuration or source repositories.

### Infrastructure Governance

Terraform provides a repeatable definition of the infrastructure and makes resource configuration easier to review, reproduce, and tear down.

### Lifecycle Management

The project considers key, secret, IAM, and resource lifecycle requirements, including the consequences of deleting keys or Vault resources that applications depend upon.

---

## Risks and Architecture Considerations

The project identified several risks that would need to be addressed in an enterprise implementation:

* Secrets accidentally committed to code or logs
* Excessive IAM privileges
* Accidental deletion of Vaults or encryption keys
* Stale credentials caused by inadequate rotation
* Incomplete audit coverage
* Insecure secret retrieval
* Exposure of sensitive Terraform state
* Sensitive information appearing in application logs

Potential production controls include automated rotation, CI/CD secret scanning, centralized SIEM monitoring, private connectivity, stronger administrative approval processes, protected Terraform state, and log redaction.

See `risks_mitigations.md` for additional detail.

---

## Compliance Considerations

The architecture supports security-control objectives commonly associated with frameworks such as:

* NIST SP 800-53
* ISO 27001
* PCI DSS
* HIPAA Security Rule

Relevant control areas include:

* Access control
* Encryption and cryptographic key management
* Protection of sensitive credentials
* Infrastructure governance
* Auditability
* Credential lifecycle management

This project demonstrates technical patterns that can support those requirements; it does not claim that the project itself constitutes compliance with any framework.

See `compliance_mapping.md` for the project-level control mapping.

---

## Cost and Operational Considerations

OCI Vault and Secrets Manager use managed cloud services, reducing the need to operate dedicated secrets-management infrastructure.

Cost and operational factors considered include:

* Number of encryption keys
* Number of stored secrets
* Secret retrieval frequency
* Logging volume and retention
* Unused or orphaned resources
* Resource lifecycle management

Terraform can help reduce resource-sprawl risk by keeping infrastructure under defined configuration and lifecycle management.

See `cost_considerations.md` for additional discussion.

---

## Lessons Learned

Several practical lessons came from implementing the project:

* OCI provider versions matter when working with Vault and KMS resources.
* Vault, encryption key, and secret resources have an explicit dependency order.
* Encryption keys must be correctly associated with the Vault before they can protect secret material.
* Terraform authentication to OCI requires establishing trust through the OCI API signing key configuration.
* WSL-based administration can introduce additional authentication and file-management considerations.

More importantly, the project reinforced that secrets management is not simply a storage problem. Identity, encryption, authorization, infrastructure provisioning, lifecycle management, and monitoring all have to work together.

See `lessonslearned.md` for implementation-specific notes.

---

## Supporting Documentation

Additional project documentation includes:

* `design_overview.md` — architecture and data-flow overview
* `risks_mitigations.md` — security risks and recommended controls
* `compliance_mapping.md` — project control mapping
* `cost_considerations.md` — cost and operational considerations
* `lessonslearned.md` — implementation lessons
* `comparison_oracle_vs_aws.md` — OCI/AWS secrets-management comparison
* `project_summary.md` — project summary
* `teardown.md` — resource teardown guidance
* `technologies.md` — technologies used

---

## Skills Demonstrated

* Oracle Cloud Infrastructure (OCI)
* OCI Vault
* OCI Key Management
* OCI Secrets Manager
* OCI IAM
* Terraform
* Infrastructure as Code
* Encryption Key Management
* Secrets Management
* Least-Privilege Access Design
* Cloud Security Architecture
* Security Risk Analysis
* Infrastructure Lifecycle Management
* Compliance Control Mapping

---

## Summary

This project demonstrates an OCI-native approach to protecting application credentials through centralized secret storage, encryption key management, IAM authorization, and Infrastructure as Code.

The implementation established the core Vault, key, secret, IAM, and Terraform components while also identifying the additional controls that would be needed for a production deployment, including automated rotation, centralized auditing, monitoring, private access, and stronger operational governance.

The result is a security architecture reference implementation that demonstrates both hands-on OCI configuration and the architectural decisions required to move from basic secret storage toward enterprise secrets governance.
