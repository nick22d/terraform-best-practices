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
