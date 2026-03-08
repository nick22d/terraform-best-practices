A module is a collection of resources that Terraform manages together. 
When you repeatedly provision collections of resources with similar configuration, such as networking resources for new development environments, you should write reusable modules to codify them. Modularizing and sharing configurations helps you standardize how you provision infrastructure and lets you quickly and predictably provision the resources you need.

Hierarchy
Every Terraform workspace includes configuration files in its root directory. Terraform refers to this configuration as the root module.

Modules you configure using module blocks are called child modules. When you apply a configuration, the root module calls the child module. As a result, Terraform adds the child module's resources to your workspace and manages them as part of the configuration.

You can configure the root module to call child modules multiple times within the same configuration. The root module can also call a child module that calls its own nested child module.
https://developer.hashicorp.com/terraform/language/modules


Create reusable modules - Build modular, reusable infrastructure components that standardize deployments across environments
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes

Pin module versions!
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.1.0"
}

---

if the module is sourced from a large monorepo with hundreds of commits, or if a CI/CD pipeline is used, or when you only need the current version, consider using a shallow clone.

---

A module is a container for multiple resources that are used together. You can use modules to create lightweight abstractions, so that you can describe your infrastructure in terms of its architecture, rather than directly in terms of physical objects.

Modules can also call other modules using a module block, but we recommend keeping the module tree relatively flat and using module composition as an alternative to a deeply-nested tree of modules, because this makes the individual modules easier to re-use in different combinations.

In principle any combination of resources and other constructs can be factored out into a module, but over-using modules can make your overall Terraform configuration harder to understand and maintain, so we recommend moderation.

A good module should raise the level of abstraction by describing a new concept in your architecture that is constructed from resource types offered by providers.

We do not recommend writing modules that are just thin wrappers around single other resource types. If you have trouble finding a name for your module that isn't the same as the main resource type inside it, that may be a sign that your module is not creating any new abstraction and so the module is adding unnecessary complexity. Just use the resource type directly in the calling module instead.

https://developer.hashicorp.com/terraform/language/modules/develop