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

GitOps workflow
GitOps is a deployment methodology that uses Git repositories as the single source of truth for both application code and infrastructure configuration. When developers want to make changes, they submit pull requests to update the Git repository. Once merged, the GitOps tooling automatically applies those changes to the target environment. The GitOps development cycle creates a fully auditable, version-controlled deployment process, where Git serves as the control plane for your entire infrastructure and application lifecycle.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/process-automation/gitops

