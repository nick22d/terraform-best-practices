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

Use a 'moved' block when moving or renaming a module without destroying/replacing existing resources

Removing a moved block is a breaking change because any configurations that refer to the old address will plan to delete the existing object instead of move it. We strongly recommend that you retain all historical moved blocks from earlier versions of your modules to preserve the upgrade path for users of any previous version.

If you do decide to remove moved blocks, proceed with caution. It can be safe to remove moved blocks when you are maintaining private modules within an organization and you are certain that all users have successfully run terraform apply with your new module version.

If you need to rename or move the same object twice, we recommend chaining moved blocks to document the full change history:
https://developer.hashicorp.com/terraform/language/modules/develop/refactoring

Terraform recognizes paths that begin with a / or drive letter as absolute paths. Terraform copies modules specified with an absolute path to the local module cache as a package. We don't recommend using absolute filesystem paths to refer to Terraform modules because doing so can couple your configuration to the filesystem layout of a particular computer.
https://developer.hashicorp.com/terraform/language/block/module

Reusable module opportunity. You have identified a subsection of your configuration that would make for a good module. If you create the same set of resources in multiple configurations, we recommend grouping those resources into a module and reusing it across your organization.
https://developer.hashicorp.com/terraform/language/state/refactor

Terraform tests let authors validate that module configuration updates do not introduce breaking changes. Tests run against test-specific, short-lived resources, preventing any risk to your existing infrastructure or state.

We recommend defining your variables and provider blocks first, at the beginning of the test file.
https://developer.hashicorp.com/terraform/language/tests

Use mocking
https://developer.hashicorp.com/terraform/language/tests/mocking

A module should have its own repo when it is:

- reusable across multiple projects/teams
- needs versioning
- has its own lifecycle
- has multiple consumers

Module repository names
The Terraform registry requires that repositories match a naming convention for all modules that you publish to the registry. Module repositories must use this three-part name terraform-<PROVIDER>-<NAME>, where <NAME> reflects the type of infrastructure the module manages and <PROVIDER> is the main provider the module uses. The <NAME> segment can contain additional hyphens, for example, terraform-google-vault or terraform-aws-ec2-instance.

Module structure
Terraform modules define self-contained, reusable pieces of infrastructure-as-code.

Use modules to group together logically related resources that you need to provision together. For example:

A networking module that defines a VPC, along with its subnets, gateway, and security groups.
An application module defining all resources required for each deployment. This stack could include web servers, databases, storage, and supported networking.
Review the module creation recommended pattern documentation and standard module structure for guidance on how to structure your modules.

Local modules
Local modules are sourced from local disk rather than a remote module registry. We recommend publishing your modules to a module registry, such as the HCP Terraform private registry, to easily version, share, and reuse modules across your organization. If you cannot use a module registry, using local modules can simplify maintaining and updating your code.

We recommend that you define child modules in the ./modules/<module_name> directory.

Repository structure
How you structure your modules and Terraform configuration in version control significantly impacts versioning and operations. We recommend that you store your actual infrastructure configuration separately from your module code.

Store each module in an individual repository. This lets you independently version each module and makes it easier to publish your modules in the private Terraform registry.

Organize your infrastructure configuration in repositories that group together logically-related resources. For example, a single repository for a web application that requires compute, networking, and database resources . By separating your resources into groups, you limit the number of resources that may be impacted by failures for any operation.

Another approach is to group all modules and infrastructure configuration into a single monolithic repository, or monorepo. For example, a monorepo may define a collection of local modules for each component of the infrastructure stack, and deploy them in the root module.

.
├── modules
│   ├── function
│   │   ├── main.tf      # contains aws_iam_role, aws_lambda_function
│   │   ├── outputs.tf
│   │   └── variables.tf
│   ├── queue
│   │   ├── main.tf      # contains aws_sqs_queue
│   │   ├── outputs.tf
│   │   └── variables.tf
│   └── vpc
│       ├── main.tf      # contains aws_vpc, aws_subnet
│       ├── outputs.tf
│       └── variables.tf
├── main.tf
├── outputs.tf
└── variables.tf

The advantage of monolithic repositories is having a single source of truth that tracks every infrastructure change. However, monolithic repositories can complicate your CI/CD automation: since any code change triggers a deployment that operates on your entire repository, your workflow must target only the modified directories. You also lose the granular access control, since anyone with repository access can modify any file in it.

If your organization requires a monolithic approach, HCP Terraform and Terraform Enterprise let you scope a workspace to a specific directory in a repository, simplifying your workflows.
https://developer.hashicorp.com/terraform/language/style