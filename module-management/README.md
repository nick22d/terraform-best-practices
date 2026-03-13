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

---

module structure !!!
https://developer.hashicorp.com/terraform/language/modules/develop/structure

---

Providers can be passed down to descendant modules in two ways: either implicitly through inheritance, or explicitly via the providers argument within a module block. These two options are discussed in more detail in the following sections.

A module intended to be called by one or more other modules must not contain any provider blocks. 

Although provider configurations are shared between modules, each module must declare its own provider requirements, so that Terraform can ensure that there is a single version of the provider that is compatible with all modules in the configuration and to specify the source address that serves as the global (module-agnostic) identifier for a provider.

If you are writing a shared Terraform module, constrain only the minimum required provider version using a >= constraint. This should specify the minimum version containing the features your module relies on, and thus allow a user of your module to potentially select a newer provider version if other features are needed by other parts of their overall configuration.

For convenience in simple configurations, a child module automatically inherits default provider configurations from its parent. This means that explicit provider blocks appear only in the root module, and downstream modules can simply declare resources for that provider and have them automatically associated with the root provider configurations.
We recommend using this approach when a single configuration for each provider is sufficient for an entire configuration.

In more complex situations there may be multiple provider configurations, or a child module may need to use different provider settings than its parent. For such situations, you must pass providers explicitly.
https://developer.hashicorp.com/terraform/language/modules/develop/providers

When we introduce module blocks, our configuration becomes hierarchical rather than flat: each module contains its own set of resources, and possibly its own child modules, which can potentially create a deep, complex tree of resource configurations.

However, in most cases we strongly recommend keeping the module tree flat, with only one level of child modules, and use a technique similar to the above of using expressions to describe the relationships between the modules:

We call this flat style of module usage module composition, because it takes multiple composable building-block modules and assembles them together to produce a larger system. Instead of a module embedding its dependencies, creating and managing its own copy, the module receives its dependencies from the root module, which can therefore connect the same modules in different ways to produce different results.

Rather than trying to write a module that itself tries to detect whether something exists and create it if not, we recommend applying the dependency inversion approach: making the module accept the object it needs as an argument, via an input variable.

We recommend validating your configuration to help capture and test for assumptions and guarantees. This helps future maintainers understand the configuration design and intent. Configuration validation returns useful information about errors earlier and in context, helping consumers more easily diagnose issues in their configurations.

As with conventional modules, we suggest using this technique only when the module raises the level of abstraction in some way, in this case by encapsulating exactly how the data is retrieved.

A common use of this technique is when a system has been decomposed into several subsystem configurations but there is certain infrastructure that is shared across all of the subsystems, such as a common IP network. In this situation, we might write a shared module called join-network-aws which can be called by any configuration that needs information about the shared network when deployed in AWS:
https://developer.hashicorp.com/terraform/language/modules/develop/composition

We recommend that modules distributed via other protocols still use the standard module structure so that they can be used in a similar way as a registry module or be published on the registry at a later time.
https://developer.hashicorp.com/terraform/language/modules/develop/publish