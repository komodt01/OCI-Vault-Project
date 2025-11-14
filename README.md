
All documentation uses the same format across clouds for consistent professional presentation.

---

## ⚙️ How It Works (High-Level)

1. **Create Vault & Keys**  
   - Keys are non-exportable  
   - Used to encrypt secrets automatically  

2. **Create Secrets in OCI Secrets Manager**  
   - Stored encrypted using Vault keys  
   - Can include API keys, passwords, tokens  

3. **Apply IAM Policies**  
   - Grant read/rotate/admin privileges to specific groups  
   - Restrict application access via Dynamic Groups  

4. **Application Retrieval**  
   - Applications retrieve secrets securely at runtime  
   - No plaintext stored in configs or images  

5. **Audit & Monitoring**  
   - All access logged  
   - Alerts enabled for suspicious activity  

---

## 🛡️ Compliance Alignment
This design aligns with:

- **NIST SP 800-53:** AC-3, SC-12, SC-13, SI-12, AU-2  
- **ISO 27001:** 5.15, 8.19, 8.28  
- **PCI DSS:** 3.5, 3.6, 7, 10  
- **HIPAA:** 164.312(a), (b), (c)  

See `compliance_mapping.md` for full details.

---

## 🧪 Testing the Implementation
To validate the configuration:

- Retrieve secrets using OCI CLI  
- Verify IAM denies unauthorized users  
- Inspect Vault and Secrets audit logs  
- Confirm rotation and lifecycle events  
- Review teardown with `teardown.md`  

---

## 📉 Cost Considerations
A serverless design keeps costs low:

- Secrets stored once (no duplication)  
- Pay-per-secret and per-API-call  
- Audit logs may be a moderate cost driver  
- Terraform (optional) prevents resource sprawl  

See `cost_considerations.md` for full breakdown.

---

## 🧠 Lessons Learned
Summaries of architectural decisions, pitfalls avoided, and alternative design paths are documented in `lessonslearned.md`.

---

## 🛑 Teardown
To avoid unnecessary consumption:
- Delete unused secrets  
- Disable or delete keys (carefully)  
- Remove IAM policies  
- Tear down test compartments  

See `teardown.md` for instructions.

---

## 🧩 Skills Demonstrated
- Oracle Cloud Infrastructure (OCI)  
- OCI Vault / Key Management  
- OCI Secrets Manager  
- IAM Policies & Access Governance  
- Audit Logging & Monitoring  
- Secret Rotation  
- Cloud Security Architecture  
- Compliance Mapping  
- Zero Plaintext Secret Handling  

---

## 📘 Summary
This project provides a production-ready blueprint for managing sensitive credentials in OCI using Vault, Secrets Manager, and IAM. It demonstrates strong cloud security architecture fundamentals, compliance-aligned design, and full lifecycle governance for secrets.

