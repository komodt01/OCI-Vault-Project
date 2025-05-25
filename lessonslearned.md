# Lessons Learned: OCI Vault Terraform Project

1. **Correct Provider Version Matters**:
   - Required OCI provider version >= 5.0.0 to use Vault and KMS resources.
2. **Vault vs Secret**:
   - Vault must be created first before storing secrets.
3. **Key Binding**:
   - Encryption key must be explicitly linked to the Vault before storing secrets.
4. **Private Key Authentication**:
   - Terraform access to OCI required manually registering an RSA keypair in user profile.
5. **Cross-Platform Access**:
   - If using WSL (Linux), OCI Console must receive public key copied manually due to file system separation.
