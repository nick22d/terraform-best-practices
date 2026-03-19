File names
We recommend the following file naming conventions:

A backend.tf file that contains your backend configuration. You can define multiple terraform blocks in your configuration to separate your backend configuration from your Terraform and provider versioning configuration.
A main.tf file that contains all resource and data source blocks.
A outputs.tf file that contains all output blocks in alphabetical order.
A providers.tf file that contains all provider blocks and configuration.
A terraform.tf file that contains a single terraform block which defines your required_version and required_providers.
A variables.tf file that contains all variable blocks in alphabetical order.
A locals.tf file that contains local values. Refer to local values for more information.
A override.tf file that contains override definitions for your configuration. Terraform loads this and all files ending with _override.tf last. Use them sparingly and add comments to the original resource definitions, as these overrides make your code harder to reason about. Refer to the override files documentation for more information.
As your codebase grows, limiting it to just these files can become difficult to maintain. If your code becomes hard to navigate due to its size, we recommend that you organize resources and data sources in separate files by logical groups. For example, if your web application requires networking, storage, and compute resources, you might create the following files:

A network.tf file that contains your VPC, subnets, load balancers, and all other networking resources.
A storage.tf file that contains your object storage and related permissions configuration.
A compute.tf file that contains your compute instances.
No matter how you decide to split your code, it should be immediately clear where a maintainer can find a specific resource or data source definition.

As your configuration grows, you may need to separate it into multiple state files. The HashiCorp Well-Architected Framework provides more guidance about configuration structure and scope.
https://developer.hashicorp.com/terraform/language/style
