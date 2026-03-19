Provider aliasing
Provider aliasing lets you define multiple provider blocks for the same Terraform provider. Potential use cases for aliases include provisioning resources in multiple regions within a single configuration. The provider meta-argument for resources and the providers meta-argument for modules specifies which provider to use.

providers.tf

provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "west"
  region = "us-west-2"
}
main.tf

resource "aws_instance" "example" {
  provider = aws.west
  # ...
}

module "aws_vpc" {
  source = "./aws_vpc"
  providers = {
    aws = aws.west
  }
}
Any provider block that does not define the alias parameter is the default provider configuration.
Always include a default provider configuration and define all of your providers in the same file.
If you define multiple instances of a provider, define the default first.
For non-default providers, define the alias as the first parameter of the provider block.
https://developer.hashicorp.com/terraform/language/style