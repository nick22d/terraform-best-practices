Local values
Local values let you reference an expression or value multiple times. Use local values sparingly, as overuse can make your code harder to understand.

For example, you can use a local value to create a suffix for the region and environment (for example, development or test), and append it to multiple resources.

locals {
  name_suffix = "${var.region}-${var.environment}"
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"

  tags = {
    Name = "web-${local.name_suffix}"
  }
}
Define local values in one of two places:

If you reference the local value in multiple files, define it in a file named locals.tf.
If the local value is specific to a file, define it at the top of that file.
As for other Terraform objects, use descriptive nouns for local value names and underscores to separate multiple words.

For more information, refer to the locals block and the Simplify Terraform configuration with locals tutorial.
https://developer.hashicorp.com/terraform/language/style