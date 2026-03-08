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