- on the terraform block
In production we recommend constraining the acceptable provider versions in the configuration's provider requirements block, to make sure that terraform init does not install newer versions of the provider that are incompatible with the configuration. https://developer.hashicorp.com/terraform/language/providers

To ensure Terraform always installs the same provider versions for a given configuration, you can use Terraform CLI to create a dependency lock file and commit it to version control along with your configuration. If a lock file is present, HCP Terraform, CLI, and Enterprise will all obey it when installing providers.https://developer.hashicorp.com/terraform/language/providers

- on the CI/CD pipeline

- locally with Docker