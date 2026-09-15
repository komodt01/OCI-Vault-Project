# Technical Case Study: OCI Vault, Secrets Management & IAM Security Architecture

## Case Study Overview

This project started with a straightforward security problem: an application needs credentials, but those credentials should not be embedded in source code, configuration files, or deployment artifacts.

For this scenario, I used Oracle Cloud Infrastructure (OCI) Vault, encryption key management, Secrets Management, IAM, and Terraform to build the core of a centralized secrets-management architecture.

The implementation was intentionally limited in scope. I built the core storage, encryption, IAM, authentication, and Infrastructure as Code components. Capabilities such as automated rotation, runtime workload retrieval, centralized monitoring, and SIEM integration were evaluated as production extensions rather than represented as completed work.

---

## The Architecture Problem

A secret such as a database password, API key, or service credential creates several security questions beyond simply where the value is stored:

- Who can access it?
- What protects it at rest?
- Who controls the encryption key?
- How is the infrastructure provisioned?
- What identity is trusted to create and manage the environment?
- What happens when the credential or encryption key must be rotated?
- How would unauthorized activity be detected?
- What happens when the environment is decommissioned?

That led me to treat secrets management as an identity, encryption, and lifecycle problem rather than simply a storage problem.

---

## Scope and Assumptions

For this project, I focused on establishing the core OCI secrets-management foundation.

### Implemented

- OCI Vault
- Vault-managed encryption key
- OCI Secrets Management
- OCI IAM policy controls
- Terraform provisioning
- OCI provider configuration
- RSA API signing key authentication
- Resource dependency management
- Project teardown

### Evaluated as Production Extensions

- Runtime application secret retrieval
- Workload identities and dynamic groups
- Automated secret rotation
- OCI Audit integration
- SIEM monitoring and alerting
- Private service access
- CI/CD secret scanning
- Protected remote Terraform state
- Administrative approval workflows

Keeping this boundary explicit was important because designing a production control and actually validating that control are not the same thing.

---

## Core Trust and Provisioning Flow

The implementation can be viewed as a chain of trust:

`Administrator → OCI Authentication → Terraform → Vault → Encryption Key → Secret → IAM Access`

Each component depends on the preceding security decisions.

### Administrator to OCI

Before Terraform could provision anything, it needed an authenticated OCI identity.

I configured RSA API signing key authentication and registered the required public key with OCI.

This was effectively the bootstrap trust relationship for the entire deployment.

If the Terraform identity is excessively privileged or its private key is compromised, the security of the resources Terraform creates can also be affected.

For a production environment, I would therefore treat the provisioning identity itself as a privileged identity requiring tightly scoped permissions and lifecycle governance.

---

## Infrastructure Provisioning

Terraform was used to define and provision the OCI resources.

One practical lesson was that the OCI provider version needed to support the Vault and key-management resources used by the project.

The resources also had an explicit dependency sequence:

`Vault → Encryption Key → Secret`

The Vault establishes the managed key environment. The encryption key provides the cryptographic protection. Secrets Management stores the protected credential material.

Terraform made these relationships explicit and provided a repeatable deployment model.

However, I would not treat Infrastructure as Code as a security control by itself. In production, I would also need to address:

- Terraform state protection
- Provider version control
- Change review
- Provisioning identity permissions
- Resource ownership
- Drift management
- Teardown governance

---

## Encryption Architecture

The encryption design separates the secret from the cryptographic mechanism protecting it.

Rather than treating the secret value and encryption key as the same security object, OCI Vault provides the key-management layer while Secrets Management provides the credential-storage layer.

Conceptually:

`Vault → Encryption Key → Protected Secret`

This separation matters operationally.

An encryption key can have dependencies extending beyond a single credential. Disabling or deleting a key without understanding those dependencies can affect every resource relying on that cryptographic material.

That is why key lifecycle management became part of the architecture rather than simply a configuration task.

---

## Identity and Authorization

Encryption protects the secret material, but encryption alone does not determine who should be allowed to access or administer it.

OCI IAM provides that authorization layer.

For the project, IAM policies were used to control access to the relevant OCI resources.

The objective was least privilege: an identity should receive only the permissions required for its role rather than broad administrative access to the entire secrets environment.

In a production application architecture, I would extend this model to workload identity.

Instead of embedding OCI user credentials in an application, the preferred pattern would be:

`Application → Workload Identity → IAM Authorization → Secret`

OCI dynamic groups and instance or resource principals could support that pattern depending on the workload.

That runtime retrieval pattern was not implemented in this project.

---

## Secret Storage

The project used OCI Secrets Management as the centralized location for sensitive credential material.

The security objective was to avoid distributing secrets through locations such as:

- Application source code
- Configuration files
- Deployment artifacts
- Container images
- Public repositories

Centralizing the secret also establishes a better foundation for access control and lifecycle management.

It does not, however, guarantee that applications will handle the retrieved value securely. A production application would still need controls preventing secrets from being written to logs, temporary files, debug output, or other inappropriate locations.

---

## Rotation

Automated secret rotation was not implemented.

For production, I would first determine what is actually being rotated because rotation affects both sides of a credential relationship.

For example, changing a database password in the secrets platform is not sufficient if the database credential itself is not updated or the consuming application cannot handle the transition.

The production rotation design would therefore need to consider:

- Credential type
- Rotation frequency
- Target-system coordination
- Application behavior
- Failure handling
- Rollback
- Availability impact
- Audit evidence

Depending on the use case, OCI Functions or other automation could participate in that workflow.

The important architectural point is that rotation is a lifecycle process, not simply a scheduled update to a stored value.

---

## Audit and Monitoring

OCI Audit/SIEM integration was not implemented in the project.

For production, I would want visibility into security-relevant events involving:

- Secret access
- Secret modification
- Key administration
- IAM policy changes
- Failed or unauthorized activity
- Destructive Vault or key operations

Those events would need to feed an appropriate monitoring and investigation capability.

I would also define which events require alerts rather than simply forwarding every event without understanding its security significance.

The architecture therefore evolves from:

`Secret Storage → Access Control`

toward:

`Secret Storage → Access Control → Audit → Detection → Investigation`

---

## Terraform State Risk

Terraform introduces its own security boundary.

State can contain infrastructure metadata and, depending on the resources and configuration, potentially sensitive information.

For a production environment, I would not rely on an unmanaged local state file.

I would evaluate a protected remote-state design with:

- Encryption
- Restricted access
- Versioning
- State locking where supported
- Backup/recovery considerations
- Administrative monitoring

The provisioning platform has to be protected with the same seriousness as the infrastructure it manages.

---

## Failure Scenarios

Several failure conditions influenced the architecture.

### Excessive IAM Permission

**Failure:** An identity receives broader secret or key permissions than required.

**Impact:** Unauthorized retrieval, modification, or destructive administration becomes possible.

**Response:** Tighten policy scope, separate administrative responsibilities, and monitor privileged operations.

### Encryption Key Unavailable

**Failure:** A key is disabled, scheduled for deletion, or otherwise unavailable.

**Impact:** Dependent secret operations or workloads may fail.

**Response:** Treat key lifecycle changes as governed operations and verify dependencies before destructive actions.

### Secret Becomes Stale

**Failure:** A long-lived credential is not rotated.

**Impact:** A compromised credential remains usable for a longer period.

**Response:** Establish a production rotation process coordinated with the consuming system.

### Terraform Authentication Compromised

**Failure:** The provisioning private key or identity is compromised.

**Impact:** An attacker may gain the ability to alter security infrastructure within the permissions assigned to that identity.

**Response:** Restrict provisioning permissions, protect authentication material, rotate credentials, and monitor administrative activity.

### Secret Appears in Logs

**Failure:** Application or automation logging records the retrieved value.

**Impact:** Centralized secrets management is bypassed because another plaintext copy now exists.

**Response:** Implement logging standards, redaction, testing, and CI/CD controls that detect inappropriate credential handling.

---

## Teardown and Lifecycle

The project also included teardown rather than treating deployment as the end of the lifecycle.

Terraform provides a repeatable mechanism for removing managed resources, but I would not assume that successful execution of `terraform destroy` alone proves that the environment is clean.

Post-teardown validation should verify:

- Secrets
- Encryption keys
- Vault resources
- Project-specific IAM policies
- Terraform artifacts
- OCI authentication keys created specifically for the project

In production, teardown would additionally require dependency validation, retention review, change approval, audit preservation, and recovery considerations.

This was particularly important for encryption keys because destructive key operations can affect dependent resources.

---

## Security Control Progression

I view the project as the first stage of a larger secrets-governance architecture.

The implemented foundation was:

`Authentication → IaC → Vault → Key → Secret → IAM`

A production evolution would add:

`Workload Identity → Runtime Retrieval → Rotation → Audit → Detection → Governance`

That distinction allows the architecture to grow without representing unimplemented capabilities as completed controls.

---

## Key Technical Decisions

The most important decisions in the project were not simply which OCI services to use.

They were:

1. **Centralize secrets rather than distribute them with applications.**
2. **Separate encryption key management from secret storage.**
3. **Use IAM as the authorization boundary around the resources.**
4. **Provision the environment through Terraform for repeatability.**
5. **Treat the Terraform provisioning identity as part of the trust architecture.**
6. **Recognize key and secret lifecycle dependencies before teardown.**
7. **Separate implemented controls from controls that would be required for production.**

---

## What I Would Add for Production

If I were taking this architecture into an enterprise environment, the next design phase would focus on:

- Workload identity and runtime secret retrieval
- Automated credential rotation
- OCI Audit integration
- SIEM monitoring and actionable alerting
- Private connectivity where appropriate
- Protected Terraform remote state
- CI/CD secret scanning
- Log redaction
- Administrative separation of duties
- Key lifecycle governance
- Recovery and availability requirements

The specific controls would depend on the application, data sensitivity, regulatory requirements, operational model, and business impact.

---

## Technical Outcome

The project established a working OCI foundation for centralized secret storage, encryption key management, IAM authorization, Terraform provisioning, and OCI API authentication.

More importantly, it demonstrated the dependencies surrounding secrets management.

A secret is ultimately protected by more than the service in which it is stored. Its security depends on the identities allowed to reach it, the encryption keys protecting it, the provisioning mechanism creating the environment, the applications consuming it, and the lifecycle controls governing it.

That broader chain of trust is the primary architecture lesson from the project.