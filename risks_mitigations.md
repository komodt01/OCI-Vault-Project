# Risks and Mitigations – OCI Vault + Secrets Manager + IAM

## 1. Secrets Exposure
**Risk:** Secrets stored incorrectly (code, GitHub, logs) may be compromised.  
**Mitigation:** Store all secrets exclusively in OCI Secrets Manager; prohibit long-lived credentials; implement CI/CD secret scanning.

## 2. Excessive IAM Privileges
**Risk:** Broad IAM policies may allow unauthorized read or rotate actions.  
**Mitigation:** Scope IAM policies to specific OCIDs; enforce least privilege; use groups and dynamic groups.

## 3. Vault or Key Deletion
**Risk:** Accidental or malicious deletion of Vault or Keys can break application decryption.  
**Mitigation:** Enable key protection policies, multi-admin approvals, and governance workflows.

## 4. Lack of Rotation
**Risk:** Stale credentials increase risk of compromise.  
**Mitigation:** Configure secret rotation schedules; enforce rotation as part of application lifecycle.

## 5. Incomplete Logging or Monitoring
**Risk:** Secret misuse or unauthorized access may go undetected.  
**Mitigation:** Enable audit logging for all Vault, Key, and Secret operations; integrate logs with SIEM.

## 6. Insecure Retrieval of Secrets
**Risk:** Applications might transmit secret values over insecure channels.  
**Mitigation:** Enforce TLS 1.2+; restrict access via private endpoints.

## 7. Terraform State Exposure
**Risk:** Sensitive metadata or ARNs in state files may be exposed.  
**Mitigation:** Store state in encrypted OCI Object Storage bucket with versioning and restricted IAM.

## 8. Overly Verbose Logging
**Risk:** Logging secret values is a common operational oversight.  
**Mitigation:** Enforce log scrubbing and redaction rules; disallow debug logging of sensitive data.
