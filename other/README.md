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

Terraform expects provider configuration in the root module.
https://developer.hashicorp.com/terraform/language/resources/configure

---

When you actually need terraform_data

Typical situations:

Trigger resource replacement when files change

Create artificial dependencies across modules

Pass derived values through state

Replace old null_resource + triggers patterns

terraform_data is essentially the modern replacement for null_resource.

We recommend using configuration management tools or other means to perform actions on the local or remote machine instead of using provisioners. https://developer.hashicorp.com/terraform/language/block/resource

---

Create multiple instances of a resource
You can use either count or the for_each block to create multiple instances of a resource. The count argument is most suitable for creating multiple instances that are identical or nearly identical. The for_each argument is most suitable for creating multiple similar instances based on attributes defined in a map or set.
https://developer.hashicorp.com/terraform/language/block/resource

---

a good use case for action_trigger is to run an ansible playbook to push config to the instance after its creation (since provisioners are not recommended):https://developer.hashicorp.com/terraform/language/block/resource

resource "aws_instance" "web" {
  #...

  lifecycle {
    action_trigger {
      events    = [ after_create ]
      actions   = [action.ansible_playbook.provision]
    }
  }
}

action "ansible_playbook" "provision" {
  #...
}

---

the data "template_file" data source was used with the template provider to render templates with variables for things like cloud-init scripts, config files, startup scripts and JSON policies.

The modern way is to use the templatefile() function

the data "local_file" data source was used with the local provider to read the contents of a file from the local filesystem so Terraform can use that content elsewhere in the configuration

The modern way is to use the file() function

Use:

file() / local_file → when you just need the file content

templatefile() / template_file → when the file contains variables

data "iam_policy_document" ??

---

Use local values in situations where you either reuse a single value in many places to let you change a value in a single place or when the value is the result of a complex expression.

you could define a locals block to create a consistent naming convention, identify your primary subnet, and to set aside environmental configuration settings:
https://developer.hashicorp.com/terraform/language/values/locals

use separate blocks to organize your related values into visually distinct blocks to make it easier for users to understand your configuration.
https://developer.hashicorp.com/terraform/language/block/locals

---

Terraform still stores the values of sensitive variables in your state. You can add the ephemeral argument to your variable configuration to omit the variable from state and plan files. 
https://github.com/hashicorp/web-unified-docs/blob/main/content/terraform/v1.14.x/docs/language/values/variables.mdx

We recommend writing the description from the point of view of a module consumer to help consumers understand why the output exists and how to use it.
https://developer.hashicorp.com/terraform/language/block/output

The output block typically does not require explicit dependencies so we recommend that when you add an explicit dependency, include a comment to explain why it's necessary.
https://developer.hashicorp.com/terraform/language/block/output

We recommend using dynamic references rather than hard-coding information about resources in another configuration. Hard-coding information requires you to manually update the configuration whenever the data changes, which can lead to errors in your configuration or your deployed resources.
https://developer.hashicorp.com/terraform/language/state/refactor

Alternatively, you can use the terraform state rm command to remove a resource from state, but we recommend using the removed block instead. This is because the removed block lets you preview the results of the operation, which makes it a safer way to remove resources.
https://developer.hashicorp.com/terraform/language/state/remove

As your infrastructure grows, managing Terraform configurations becomes increasingly complex. Stacks are a powerful configuration layer in HCP Terraform that simplifies managing your infrastructure modules and then repeatedly deploying that infrastructure. Stacks replace Terraform's traditional root module structure with a new component-based architecture built on top of your Terraform modules. Stacks let you to provision and coordinate your infrastructure lifecycle at scale, offering an organized and reusable approach that expands upon infrastructure as code (IaC).
https://developer.hashicorp.com/terraform/language/stacks

Before writing your component configuration, we recommend planning and designing how you want to use your new Stack:

Understand your existing infrastructure and break it down into manageable components.
Understand how you want to deploy the infrastructure your Stack defines.
Align your Stack’s component organization with your deployment strategy.
You aim to create a flexible and reusable component configuration, enabling you to manage your infrastructure lifecycle efficiently.

We recommend structuring your Stacks along technical boundaries to keep them modular and manageable. For example, you can create a dedicated Stack for shared services, such as networking infrastructure for VPCs, subnets, or routing tables, and separate Stacks for application components that consume those shared services. This separation lets you manage shared services independently while passing information between Stacks. 
https://developer.hashicorp.com/terraform/language/stacks/design

We recommend designing your Stack before you begin writing your configuration files.

All of your Stack’s configuration files must use the .tfcomponent.hcl file type. You can set up your component configuration into multiple files as in traditional Terraform configurations. For example, you can have variables.tfcomponent.hcl, providers.tfcomponent.hcl, and we recommend creating one root-level file for your component blocks, such as components.tfcomponent.hcl.
https://developer.hashicorp.com/terraform/language/stacks/component/config

Define provider configurations in the root module of your Terraform configuration. Child modules receive their provider configurations from their parent modules, so we strongly recommend against defining provider blocks in child modules.
https://developer.hashicorp.com/terraform/language/block/provider

To create multiple configurations for a given provider, include multiple provider blocks with the same provider name, then add the alias argument to each additional provider configuration to give it a unique identifier.

provider "exampleName" {
  region = "us-east-1"
}

provider "exampleName" {
  alias  = "west"
  region = "us-west-1"
}

The version argument in provider configurations is deprecated, and Terraform will remove it in a future version. Instead, declare provider version constraints in the terraform block's required_providers block.
https://developer.hashicorp.com/terraform/language/block/provider

Basic provider configuration
The following example configures the google provider with a specific project and region. In the terraform block's required_providers block you define the provider version you want to use, where to source the provider from, and the local name of the provider:

terraform.tf

terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 4.0"
    }
  }
}

You can configure the provider with the provider block in the root module of your configuration.

main.tf

provider "google" {
  project = "acme-app"
  region  = "us-central1"
}
https://developer.hashicorp.com/terraform/language/block/provider

Pass provider configurations to a child module
When you define a provider in your root module, Terraform implicitly passes that provider configuration to any child modules to ensure all modules use the same configuration.

In the following example, the root module defines an AWS provider configuration and Terraform implicitly passes that configuration to a child module:

main.tf

provider "aws" {
  region = "us-west-2"
}

module "vpc" {
  source = "./modules/vpc"
}

The child module uses the provider configuration passed from the root module:

modules/vpc/main.tf

terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "Main VPC"
  }
}

The aws_vpc resource inherits the same AWS provider configuration from the root module, and Terraform creates the aws_vpc resource in the us-west-2 region.

Child modules do not inherit provider source or version requirements, so you must explicitly define those within a child module. Learn more about inheriting providers in modules.

Using an alternate provider configuration in a child module
To use an aliased provider configuration in a child module, the child module must declare the alias using the configuration_aliases argument in the required_providers block.

In the following example, the root module passes the aws.west alias to the web-server child module:

main.tf

provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "west"
  region = "us-west-2"
}

module "web-server" {
  source = "./modules/web-server"
  providers = {
    aws.west = aws.west
  }
}

The following configuration_aliases argument declares that the child module expects to receive a provider configuration it can reference as aws.west. Without this declaration, Terraform raises an error when the module tries to reference aws.west in its resources.

modules/web-server/main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
      configuration_aliases = [aws.west]
    }
  }
}

data "aws_ami" "amazon_linux" {
  provider = aws.west

  #...
}

Modules have special requirements for providers, refer to Providers within modules to learn more.
https://developer.hashicorp.com/terraform/language/block/provider