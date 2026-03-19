- on the terraform block
In production we recommend constraining the acceptable provider versions in the configuration's provider requirements block, to make sure that terraform init does not install newer versions of the provider that are incompatible with the configuration. https://developer.hashicorp.com/terraform/language/providers

Use Terraform version constraints in a collaborative environment to ensure that everyone is using a specific Terraform version, or using at least a minimum Terraform version that has behavior expected by the configuration. https://developer.hashicorp.com/terraform/language/block/terraform

To ensure Terraform always installs the same provider versions for a given configuration, you can use Terraform CLI to create a dependency lock file and commit it to version control along with your configuration. If a lock file is present, HCP Terraform, CLI, and Enterprise will all obey it when installing providers.https://developer.hashicorp.com/terraform/language/providers

- on the CI/CD pipeline

- locally with Docker

The version argument in provider configurations is deprecated, and Terraform will remove it in a future version. Instead, declare provider version constraints in the terraform block's required_providers block.
https://developer.hashicorp.com/terraform/language/block/provider

The required_version configuration applies only to the version of Terraform CLI and not versions of provider plugins. 
https://developer.hashicorp.com/terraform/language/block/terraform

In professional Terraform setups you should pin both:

The Terraform CLI version

The provider versions

This ensures deterministic, reproducible infrastructure deployments.

- pin module versions as well