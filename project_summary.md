# Project Summary – OCI Vault + Secrets Manager + IAM Policy Enforcement

## 1. Purpose
This project demonstrates how to secure sensitive application credentials in Oracle Cloud Infrastructure (OCI) using OCI Vault, Keys, Secrets Manager, and IAM policy controls. The design focuses on enforcing least privilege, protecting secret material, applying cryptographic best practices, and maintaining complete auditability for regulated environments.

## 2. Business Context
Organizations must protect sensitive credentials such as database passwords, API keys, and tokens while maintaining compliance with frameworks like NIST 800-53, ISO 27001, PCI DSS, and HIPAA. This project provides a secure pattern for storing, rotating, and retrieving secrets in cloud-native applications deployed on OCI.

## 3. High-Level Architecture
- OCI Vault provides secure, non-exportable keys.
- OCI Secrets Manager stores and encrypts credentials using Vault-managed keys.
- IAM Policies restrict who can create, read, rotate, or manage secrets.
- Audit Logs capture secret retrieval, rotation events, and administrative changes.
- Applications retrieve secrets at runtime using IAM instance principals.

## 4. Outcomes
- Secure secret storage with strong encryption.
- Enforced least privilege via IAM policy scoping.
- Full audit traceability of all access and administrative operations.
- A cloud-native, compliant, and production-ready secret governance pattern.

## 5. Skills Demonstrated
OCI Vault · KMS · Secrets Manager · IAM · Encryption Key Management · Audit Logging · Access Control · Cloud Security Architecture · Compliance Mapping · Infrastructure Governance

