# Project Summary: OCI Vault + Secrets Manager with Terraform

## Business Case
In highly regulated environments like finance and healthcare, securely managing application secrets (API keys, passwords) is critical to preventing breaches and ensuring compliance. This project demonstrates how Oracle Cloud Infrastructure (OCI) Vault and Secrets Manager, deployed using Terraform, provide a scalable and automated solution to manage secrets securely.

## Problem Statement
Many enterprises hardcode secrets into source code or store them in configuration files, increasing the risk of compromise. The challenge is to securely store, rotate, and audit secrets access in cloud-native environments.

## Project Definition
Provision a vault, encryption key, and store a secret using OCI Vault and Secrets Manager via Terraform automation.

## Goals
- Automate secure vault and key provisioning using Terraform.
- Store a secret value with encryption at rest and in transit.
- Maintain infrastructure-as-code for repeatable deployments.

## Resources Used
- OCI Vault
- OCI KMS Key
- OCI Secrets Manager
- Terraform
- RSA keypair for authentication

## Methodology
1. Configure Terraform provider with OCI credentials.
2. Define Terraform resources for vault, key, and secret.
3. Run `terraform apply` to deploy all resources.
4. Validate in the OCI Console.

## Results
- Vault and encryption key successfully provisioned.
- Secret value securely stored with BASE64 encoding.
- Automation ensures reproducibility and compliance.

## Recommendations
- Integrate secret access with IAM policies for fine-grained control.
- Automate secret rotation using OCI Functions.
- Extend project to include multi-region high availability.

