The workflow for importing single resources or small batches of resources works best when you can easily access unique infrastructure resource IDs and other attributes from your cloud provider. In this workflow, manually write import and resource blocks and run the terraform apply command to import the resources.

Alternatively, you can write only the import block and run the terraform plan command with the generate-config-out flag to generate the resource blocks. Refer to Import an existing resource for more information.
https://developer.hashicorp.com/terraform/language/import

We recommend manually writing the resource block when you know how to configure all or most of the resource's arguments. Use generated configuration when importing multiple resources or a single complex resource that you do not already have the configuration for.

You can remove import blocks from your configuration after importing resources, but we recommend keeping them as an historical artifact.
https://developer.hashicorp.com/terraform/language/import/single-resource

The import block imports existing infrastructure resources into Terraform. You can add an import block to any Terraform configuration file, but we recommend either creating an imports.tf file for all import configurations or placing each import block beside the destination resource block.
https://developer.hashicorp.com/terraform/language/block/import



