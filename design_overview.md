# Design Overview – OCI Vault + Secrets Manager + IAM

## 1. Architecture Summary
The architecture implements secure secret storage and strong access control using OCI-native services:

- **OCI Vault:** Stores and manages encryption keys.
- **OCI Secrets Manager:** Stores encrypted secrets.
- **IAM Policies:** Enforce least privilege and role-based access.
- **Audit Logs:** Provide full traceability of key and secret operations.

The design ensures that secrets are never exposed in plaintext and that key material remains non-exportable.

---

## 2. Data Flow
1. Administrator creates a Vault and non-exportable key.
2. Secrets Manager stores secrets encrypted with the Vault key.
3. Applications retrieve secrets using IAM instance principals.
4. Audit logs record retrieval and rotation events.

---

## 3. Security Controls
- Encryption in transit (TLS 1.2+)
- Encryption at rest (Vault keys)
- IAM-based authorization
- Key rotation and secret rotation
- Compartment and VCN boundaries
- Audit logging of all operations

---

## 4. Architectural Benefits
- Least privilege access enforcement
- Complete auditability
- Zero plaintext storage
- Strong compliance alignment
- Secret lifecycle governance

---

## 5. Deployment Model
This architecture can be deployed manually or via Terraform.  
Terraform enables versioning, repeatable deployments, and safe teardown using `teardown.md`.

