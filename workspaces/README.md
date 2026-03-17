Within your Terraform configuration, you may include the name of the current workspace using the ${terraform.workspace} interpolation sequence. This can be used anywhere interpolations are allowed.

Referencing the current workspace is useful for changing behavior based on the workspace. For example, for non-default workspaces, it may be useful to spin up smaller cluster sizes.
https://developer.hashicorp.com/terraform/language/state/workspaces