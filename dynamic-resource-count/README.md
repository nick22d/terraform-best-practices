Dynamic resource count
The for_each and count meta-arguments let you create multiple resources from a single resource block depending on run-time conditions. You can use these meta-arguments to make your code flexible and reduce duplicate resource blocks. If the resources are almost identical, use count. If some of the arguments need distinct values that you cannot derive from an integer, use for_each.

The for_each meta-argument accepts a map or set value, and Terraform will create an instance of that resource for each element in the value you provide. In the following example, Terraform creates an aws_instance for each of the strings defined in the web_instances variable: "ui", "api", "db" and "metrics". The example uses each.key to give each instance a unique name. The web_private_ips output uses a for expression to create a map of instance names and their private IP addresses, while the web_ui_public_ip output addresses the instance with the key "ui" directly.

variable "web_instances" {
  type        = list(string)
  description = "A list of instances for the web application"
  default = [
    "ui",
    "api",
    "db",
    "metrics"
  ]
}
resource "aws_instance" "web" {
  for_each = toset(var.web_instances)
  ami           = data.aws_ami.webapp.id
  instance_type = "t3.micro"
  tags = {
    Name = "web_${each.key}"
  }
}
output "web_private_ips" {
  description = "Private IPs of the web instances"
  value = {
    for k, v in aws_instance.web : k => v.private_ip
  }
}
output "web_ui_public_ip" {
  description = "Public IP of the web UI instance"
  value       = aws_instance.web["ui"].public_ip
}
The above example will create the following output:

web_private_ips = {
  "api" = "172.31.25.29"
  "db" = "172.31.18.33"
  "metrics" = "172.31.26.112"
  "ui" = "172.31.20.142"
}
web_ui_public_ip = "18.216.208.182"
Refer to the for_each meta-argument documentation for more examples.

The count meta-argument lets you create multiple instances of a resource from a single resource block. Refer to the count meta-argument documentation for examples.

A common practice to conditionally create resources is to use the count meta-argument with a conditional expression. In the following example, Terraform will only create the aws_instance if var.enable_metrics is true.

variable "enable_metrics" {
  description = "True if the metrics server should be deployed"
  type        = bool
  default     = true
}

resource "aws_instance" "web" {
  count = var.enable_metrics ? 1 : 0

  ami           = data.aws_ami.webapp.id
  instance_type = "t3.micro"
  ##...
}
Meta-arguments simplify your code but add complexity, so use them in moderation. If the effect of the meta-argument is not immediately obvious, use a comment for clarification.

To learn more about these meta-arguments, refer to the for_each and count documentation.
https://developer.hashicorp.com/terraform/language/style