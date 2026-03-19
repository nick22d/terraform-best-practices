Secrets management
If you do not configure remote state storage, the Terraform CLI stores the entire state in plaintext on the local disk. State can include sensitive data, such as passwords and private keys. HCP Terraform and Terraform Enterprise provide state encryption through HashiCorp Vault.

If you use HCP Terraform or Terraform Enterprise, we recommend the following:

When using Terraform Enterprise, define and enforce a Sentinel policy to prevent use of the local_exec provisioner or external data sources.
When using HCP Terraform or Terraform Enterprise, use dynamic provider credentials to avoid using long-lived static credentials.
If you use Terraform Community Edition, we recommend the following:

Configure provider credentials using provider-specific environment variables.
Access secrets from a secrets management system such as HashiCorp Vault with the Terraform Vault provider. Be aware that Terraform will still write these values in plaintext to your state file.
If you use a custom CI/CD pipeline, review your CI/CD tool's best practices for managing sensitive values. Most tools let you access sensitive values as environment variables. For more information, refer to your CI/CD documentation.

Using secrets in GitHub Actions
Gitlab pipeline security
Integrate Vault into your CI/CD pipeline
https://developer.hashicorp.com/terraform/language/style