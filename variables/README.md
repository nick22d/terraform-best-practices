For readability, and to avoid shell escaping, we recommend setting complex variable values with variable definition files.
https://developer.hashicorp.com/terraform/language/values/variables

Variables
While variables make your modules more flexible, overusing variables can make code difficult to understand. When deciding whether to expose a variable for a resource setting, consider whether that parameter will change between deployments.

Define a type and a description for every variable.

If the variable is optional, define a reasonable default.

For sensitive variables, such as passwords and private keys, set the sensitive parameter to true. Remember that Terraform will still store this value in plain text in its state, but it will not display it when you run terraform plan or terraform apply. Refer to secrets management for more information on how to securely handle sensitive values.

Use input variable validation to create additional rules for your variable values in addition to Terraform's type validation. Only use variable validation when your variable values have uniquely restrictive requirements. For example, if your Terraform configuration requires two web instances, add a validation block to enforce it:

variable "web_instance_count" {
  type        = number
  description = "Number of web instances to deploy. This application requires at least two instances."

  validation {
    condition     = var.web_instance_count > 1
    error_message = "This application requires at least two web instances."
  }
}
We recommend following a consistent order for variable parameters:

Type
Description
Default (optional)
Sensitive (optional)
Validation blocks
https://developer.hashicorp.com/terraform/language/style