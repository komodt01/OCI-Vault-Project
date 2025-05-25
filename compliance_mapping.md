# Compliance Mapping: OCI Vault Project

| Control Category     | Description                                       | OCI Implementation                      |
|----------------------|---------------------------------------------------|------------------------------------------|
| Encryption at Rest   | Secrets must be encrypted at rest                | OCI Vault with KMS key (AES-256)         |
| IAM Controls         | Role-based access to secrets                     | OCI IAM user and policy-based access     |
| Auditing             | Access to secrets must be logged                 | OCI Audit service (not covered here)     |
| Secrets Rotation     | Secrets should be rotated regularly              | Not implemented; possible via OCI Funcs  |
| Terraform Governance | Infrastructure as code for auditability         | All resources declared via `.tf` files   |
