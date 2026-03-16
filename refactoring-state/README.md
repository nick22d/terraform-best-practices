https://developer.hashicorp.com/terraform/language/state/refactor

You can use the terraform graph command to help visualize the relationships between the resources in your configuration and identify any dependencies.

When you refactor your Terraform configuration, we recommend that you recreate stateless resources in a new Terraform configuration if you can do so without incurring downtime or extra cost.

There are two common approaches to migrating resources between two Terraform state files:

Remove and import: Explicitly remove resources from one Terraform state file, then add it to another. We recommend this approach because the removed and import blocks help keep a record of the configuration history.
Move resources directly to a new state file: Use the terraform state mv command to move resources into a different state file. This is a legacy command.

For each resource you want to migrate, replace the resource block with a removed block to remove the resource from your state without destroying it. These removed blocks can exist anywhere in your configuration, but we recommend your organization standardizes where to declare these blocks to help with future maintenance. One common approach is defining the removed block in the file that previously contained the corresponding resource block.

For each added resource, add an import block to add the resource to your state without recreating it. These import blocks can exist anywhere in your configuration, but we recommend you standardize on location in your organization to help with future maintenance. One common practice is to define the import block in the same file you add the resource block to.

We recommend that you use the removed and import blocks documented above for any new migrations.