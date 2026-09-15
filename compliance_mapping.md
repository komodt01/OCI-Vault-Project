# Security Control Mapping – OCI Vault Project

## Purpose

This document maps the security capabilities addressed by the OCI Vault project to common security control objectives.

The project is not intended to demonstrate full compliance with NIST SP 800-53, ISO 27001, PCI DSS, HIPAA, or any other framework. Instead, it demonstrates technical controls that can contribute to a broader compliance program.

The mapping also distinguishes between controls that were implemented in the project and controls that would be required when extending the architecture into production.

---

## Control Coverage

| Control Area               | Security Objective                                                                   | OCI Approach                              | Project Status                 |
| -------------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------- | ------------------------------ |
| Secret Storage             | Prevent sensitive credentials from being stored in application code or configuration | OCI Secrets Manager                       | **Implemented**                |
| Encryption at Rest         | Protect stored secret material                                                       | OCI Vault-managed encryption key          | **Implemented**                |
| Key Management             | Separate cryptographic key management from secret storage                            | OCI Vault and Key Management              | **Implemented**                |
| Access Control             | Restrict access to secret and key resources                                          | OCI IAM policies                          | **Implemented**                |
| Least Privilege            | Limit identities to required permissions                                             | Scoped OCI IAM policy design              | **Implemented / Demonstrated** |
| Infrastructure Governance  | Maintain repeatable infrastructure configuration                                     | Terraform                                 | **Implemented**                |
| Authentication             | Secure administrative/API access to OCI                                              | OCI API signing key / RSA authentication  | **Implemented**                |
| Audit Logging              | Record security-relevant administrative and access activity                          | OCI Audit and logging services            | **Production Extension**       |
| Security Monitoring        | Detect suspicious secret or key activity                                             | Centralized monitoring / SIEM integration | **Production Extension**       |
| Secret Rotation            | Reduce exposure from long-lived credentials                                          | Rotation workflow or automation           | **Production Extension**       |
| Private Access             | Reduce unnecessary public network exposure                                           | OCI private connectivity controls         | **Production Extension**       |
| CI/CD Secret Detection     | Prevent credentials from entering source repositories or build artifacts             | Secret scanning within CI/CD              | **Production Extension**       |
| Terraform State Protection | Protect infrastructure state and metadata                                            | Encrypted, access-controlled remote state | **Production Extension**       |
| Log Protection             | Prevent sensitive values from being written to operational logs                      | Redaction and logging standards           | **Production Extension**       |
| Administrative Governance  | Control high-impact key and Vault operations                                         | Approval and governance workflows         | **Production Extension**       |

---

## Framework Alignment

The controls demonstrated or considered by this architecture can support security requirements commonly found in:

* **NIST SP 800-53** — access control, auditability, cryptographic protection, system integrity, and credential management
* **ISO/IEC 27001** — access control, cryptography, logging, secure configuration, and information protection
* **PCI DSS** — protection of sensitive data, access restriction, cryptographic key management, logging, and authentication
* **HIPAA Security Rule** — access control, audit controls, integrity protections, and transmission security

Exact framework applicability depends on the organization's systems, data classification, implementation, operating procedures, and compliance scope.

---

## Implementation Boundary

The project directly implemented the core secrets-management foundation:

`OCI Authentication → Vault → Encryption Key → Secret → IAM Access`

Audit integration, automated rotation, SIEM monitoring, private connectivity, CI/CD scanning, hardened remote Terraform state, and operational governance were evaluated as production architecture requirements but were not implemented as part of this project.

This distinction is intentional. Implementing a cloud security service does not by itself establish compliance. Compliance depends on the combination of technical controls, operational processes, governance, evidence, and continuous oversight.

---

## Summary

The OCI Vault project demonstrates several foundational controls for secure credential management, particularly centralized secret storage, encryption, IAM authorization, and Infrastructure as Code.

The architecture also identifies the additional detective, lifecycle, network, and governance controls that would be necessary to evolve the implementation into a more complete enterprise secrets-management capability.
