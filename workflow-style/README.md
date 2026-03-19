Workflow style
This section reviews standards that enable predictable and secure Terraform workflows, such as:

Pin your Terraform, provider, and module versions.
Name your module repositories using this three-part name terraform-<PROVIDER>-<NAME> when using the HCP Terraform registry.
Store local modules at ./modules/<module_name>.
Use the tfe_outputs data source or provider-specific data sources to share state between two state files.
Protect credentials by using dynamic provider credentials or a secrets manager such as HashiCorp Vault.
Write tests for your modules.
Use policy enforcement on HCP Terraform to set guardrails for infrastructure operations.
https://developer.hashicorp.com/terraform/language/style