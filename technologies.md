# Technologies – OCI Vault + Secrets Manager + IAM

## OCI Vault (KMS)
**What it is:**  
Cloud-native key management service providing secure, non-exportable keys.

**Why used:**  
To encrypt all secrets with strong, enterprise-grade key controls.

**How it works here:**  
Keys created in the Vault encrypt secrets stored in OCI Secrets Manager.

---

## OCI Secrets Manager
**What it is:**  
A managed service for storing credentials, tokens, and sensitive values.

**Why used:**  
Prevents hardcoding secrets, supports rotation, and integrates with IAM.

**How it works here:**  
Secrets are encrypted with Vault keys and retrieved securely at runtime.

---

## OCI IAM
**What it is:**  
Identity and policy system for controlling access to OCI resources.

**Why used:**  
Prevents unauthorized access to secrets and Vault operations.

**How it works here:**  
IAM groups, policies, and dynamic groups restrict read/rotate/admin rights.

---

## OCI Audit Logs
**What it is:**  
Cloud-native auditing for all API operations.

**Why used:**  
Ensures compliance, traceability, and forensic capability.

**How it works here:**  
Logs track secret retrieval, rotation, key creation, policy changes.

---

## Terraform (Optional Use)
**What it is:**  
Infrastructure-as-code tool.

**Why used:**  
Provides reproducible deployment, avoids misconfigurations.

**How it works here:**  
Defines Vault, Keys, Secrets, and IAM policies with version control.


