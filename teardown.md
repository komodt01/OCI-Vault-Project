# Teardown and Resource Lifecycle – OCI Vault Project

## Purpose

This document describes the teardown process used for the OCI Vault Terraform project and the checks that should be performed after Terraform attempts to remove the deployed resources.

Vaults, encryption keys, and secrets have security and lifecycle dependencies, so teardown should not be treated as simply deleting infrastructure without first understanding those relationships.

---

## Terraform Teardown

For resources managed by the Terraform configuration, initiate teardown using:

```bash
terraform destroy -var-file="terraform.tfvars"
```

Review the Terraform execution plan before confirming the destroy operation.

The goal is to verify which resources Terraform intends to remove before allowing destructive changes to proceed.

---

## Resource Dependency Considerations

The project contains an important dependency chain:

`Vault → Encryption Key → Secret`

Deletion of security resources should therefore be approached carefully.

Before removing resources in a production environment, I would verify:

* Whether applications still depend on the secret
* Whether encrypted information depends on the key
* Whether retention requirements apply
* Whether audit or compliance evidence must be preserved
* Whether another environment or service depends on the Vault
* Whether the resource is subject to OCI deletion or recovery lifecycle behavior

For this project, teardown is intended for the project environment rather than a production workload.

---

## Post-Teardown Verification

After Terraform completes, verify the OCI environment rather than assuming that every resource has been removed.

Check the OCI Console for:

* Remaining secrets
* Vault resources
* Encryption keys
* IAM policies created specifically for the project
* Other resources associated with the project

If a resource remains, determine whether it is still tracked by Terraform, has an OCI lifecycle restriction, or requires additional cleanup.

---

## Terraform Local Files

Terraform may create local working files containing infrastructure configuration or state information.

Examples include:

* `.terraform/`
* Terraform state files
* Variable files

These files should be handled according to their sensitivity.

Terraform state and variable files should not be committed to public source repositories if they contain sensitive information.

Local project artifacts that are no longer required can be removed after confirming that they are not needed for recovery, troubleshooting, or continued infrastructure management.

---

## OCI Authentication Keys

The project used RSA API signing keys for Terraform authentication to OCI.

If authentication keys were created specifically for the project and are no longer required, remove the corresponding OCI API key registration and securely remove the local private-key material.

Example local cleanup for project-specific keys:

```bash
rm -f ~/.oci/vault.pem ~/.oci/vault_public.pem
```

Do not remove authentication keys that are still used by other OCI tooling or environments.

---

## Production Considerations

A production teardown process would require stronger governance than a project environment.

I would expect controls such as:

* Change approval
* Dependency validation
* Resource ownership confirmation
* Data-retention review
* Key lifecycle review
* Audit evidence preservation
* Recovery planning
* Post-change validation

Encryption keys deserve particular attention because deleting or disabling a key can affect every workload or secret dependent on that cryptographic material.

---

## Summary

Terraform provides a repeatable way to remove infrastructure, but successful execution of `terraform destroy` should not be treated as the entire lifecycle process.

Secure teardown requires understanding resource dependencies, verifying the resulting cloud environment, protecting or removing local infrastructure artifacts appropriately, and revoking authentication material that is no longer required.
