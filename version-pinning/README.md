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

Version pinning
To prevent provider and module upgrades from introducing unintentional changes to your infrastructure, use version pinning.

Specify provider versions using the required_providers block. Terraform version constraints support a range of accepted versions.

Pin modules to a specific major and minor version as shown in the example below to ensure stability. You can use looser restrictions if you are certain that the module does not introduce breaking changes outside of major version updates.

We also recommend that you set a minimum required version of the Terraform binary using the required_version in your terraform block. This requires all operators to use a Terraform version that has all of your configuration's required features.

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.34.0"
    }
  }

  required_version = ">= 1.7"
}
The above example pins the version of the hashicorp/aws provider to version 5.34.0, and requires that operators use Terraform 1.7 or newer.

For modules sourced from a registry, use the version parameter in the module block to pin the version. For local modules, Terraform ignores the version parameter.

module "vault_starter" {
  source  = "hashicorp/vault-starter/aws"
  version = "1.0.0"
  ##...
}
https://developer.hashicorp.com/terraform/language/style

The version argument in provider configurations is deprecated, and Terraform will remove it in a future version. Instead, declare provider version constraints in the terraform block's required_providers block.
https://developer.hashicorp.com/terraform/language/block/provider

required_version
Specifies which version of the Terraform CLI is allowed to run the configuration.

terraform {
  required_version = "<Terraform version>"
  # . . .
}

The required_version configuration applies only to the version of Terraform CLI and not versions of provider plugins. 

required_providers
Specifies all provider plugins required to create and manage resources specified in the configuration.

terraform {
  required_providers {
    <PROVIDER> {}
  }
  # . . .
}

Each local provider name maps to a source address and a version constraint. Refer to each Terraform provider’s documentation in the public Terraform Registry, or your private registry, for instructions on how to configure attributes in the required_providers block.
https://developer.hashicorp.com/terraform/language/block/terraform

Best practices
We recommend implementing the following best practices when configuration version constraints.

Module versions
Require specific versions to ensure that updates only happen when convenient to you when your infrastructure depends on third-party modules.

Specify version ranges when your organization consistently uses semantic versioning for modules it maintains.

Specify version ranges when your organization follows a well-defined release process that avoids unwanted updates.

Terraform core and provider versions
Reusable modules should constrain only their minimum allowed versions of Terraform and providers, such as >= 0.12.0. This helps avoid known incompatibilities, while allowing the user of the module flexibility to upgrade to newer versions of Terraform without altering the module.

Root modules should use a ~> constraint to set both a lower and upper bound on versions for each provider they depend on.
https://developer.hashicorp.com/terraform/language/expressions/version-constraints