# Cost Considerations – OCI Vault + Secrets Manager

## 1. Serverless, Managed Architecture
Costs scale with usage—no compute infrastructure required.

## 2. Key and Secret Storage
- Vault: billed per key per month.
- Secrets Manager: billed per secret + API calls.

This design avoids cost duplication by storing each secret only once.

## 3. Secret Retrieval
Applications incur a small per-call cost to retrieve secrets.  
This is minimal for standard workloads.

## 4. Logging & Audit
Audit logs may be a moderate cost driver depending on:
- Retention period
- Access frequency

Retention policies can reduce long-term costs.

## 5. Terraform Governance
Terraform helps prevent:
- Unused key proliferation
- Unnecessary secrets
- Orphaned Vault resources

## 6. Cost Risks
- Excessive keys (risk: unnecessary Vault charges)
- High-frequency secret retrieval
- Large-scale log retention

Mitigation: resource tagging, lifecycle reviews, and right-sizing Vault assets.
