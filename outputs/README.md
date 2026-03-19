Outputs
Output values let you expose data about your infrastructure on the command line and make it easy to reference in other Terraform configurations. Like you would for variables, provide a description for each output.

We recommend that you use the following order for your output parameters:

Description
Value
Sensitive (optional)
Every variable and output requires a unique name. For consistency and readability, we recommend that you use a descriptive noun and separate words with underscores.

variable "db_disk_size" {
 type        = number
 description = "Disk size for the API database"
 default     = 100
}

variable "db_password" {
 type        = string
 description = "Database password"
 sensitive   = true
}

output "web_public_ip" {
 description = "Public IP of the web instance"
 value       = aws_instance.web.public_ip
}
https://developer.hashicorp.com/terraform/language/style