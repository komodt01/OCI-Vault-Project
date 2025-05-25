# OCI Vault vs AWS Secrets Manager

| Feature                    | Oracle Cloud Infrastructure (OCI) Vault       | AWS Secrets Manager                  |
|---------------------------|-----------------------------------------------|--------------------------------------|
| Service Integration       | Deeply integrated with OCI KMS                | Integrated with KMS & IAM            |
| Secret Rotation           | Manual (OCI Functions can be scripted)        | Native support with Lambda rotation  |
| Terraform Provider        | `oracle/oci`                                  | `hashicorp/aws`                      |
| API Support               | REST APIs + SDKs                              | REST APIs + SDKs                     |
| Multi-Region Replication  | Manual setup                                  | Automatic in some tiers              |
| Pricing Model             | Based on usage & encryption key calls         | Flat monthly + API call charges      |

**Conclusion:** OCI Vault is highly secure and native to Oracle workloads. AWS Secrets Manager offers more built-in automation features, but both platforms enable scalable secret management with Terraform.
