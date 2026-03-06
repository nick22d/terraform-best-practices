enforce naming convention by:

- validating variables
- built-in functions when naming tags and values

scalable infrastructure??

separation of concerns

Define provider configurations in the root module of your Terraform configuration. Child modules receive their provider configurations from their parent modules, so we strongly recommend against defining provider blocks in child modules.
https://developer.hashicorp.com/terraform/language/block/provider

consider using multiple provider blocks when managing multi-region, multi-account or cross-account infrastructure.

provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "west"
  region = "us-west-2"
}

You can choose to omit this block entirely if you don't need to configure any provider-specific arguments.
https://developer.hashicorp.com/terraform/language/block/provider
