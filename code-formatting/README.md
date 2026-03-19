Code formatting
The Terraform parser allows you some flexibility in how you lay out the elements in your configuration files, but the Terraform language also has some idiomatic style conventions which we recommend users always follow for consistency between files and modules written by different teams.

Indent two spaces for each nesting level

When multiple arguments with single-line values appear on consecutive lines at the same nesting level, align their equals signs:

ami           = "abc123"
instance_type = "t2.micro"
When both arguments and blocks appear together inside a block body, place all of the arguments together at the top and then place nested blocks below them. Use one blank line to separate the arguments from the blocks.

Use empty lines to separate logical groups of arguments within a block.

For blocks that contain both arguments and "meta-arguments" (as defined by the Terraform language semantics), list meta-arguments first and separate them from other arguments with one blank line. Place meta-argument blocks last and separate them from other blocks with one blank line. Refer to dynamic resource count for more information on meta-arguments.

resource "aws_instance" "example" {
  # meta-argument first
  count = 2

  ami           = "abc123"
  instance_type = "t2.micro"

  network_interface {
    # ...
  }

  # meta-argument block last
  lifecycle {
    create_before_destroy = true
  }
}
Top-level blocks should always be separated from one another by one blank line. Nested blocks should also be separated by blank lines, except when grouping together related blocks of the same type (like multiple provisioner blocks in a resource).

Avoid grouping multiple blocks of the same type with other blocks of a different type, unless the block types are defined by semantics to form a family. (For example: root_block_device, ebs_block_device and ephemeral_block_device on aws_instance form a family of block types describing AWS block devices, and can therefore be grouped together and mixed.)

The terraform fmt command formats your Terraform configuration to a subset of the above recommendations. By default, the terraform fmt command will only modify your Terraform code in the directory that you execute it in, but you can include the -recursive flag to modify code in all subdirectories as well.

We recommend that you run terraform fmt before each commit to version control. You can use mechanisms such as Git pre-commit hooks to automatically run this command each time you commit your code.

If you use Microsoft VS Code, use the Terraform VS Code extension to enable features such as syntax highlighting and validation, automatic code formatting, and integration with HCP Terraform. If your development environment or text editor supports the Language Server Protocol, you can use the Terraform Language Server to access most of the VS Code extension features.
https://developer.hashicorp.com/terraform/language/style