.gitignore
Define a .gitignore file for your repository to exclude files that you should not publish to version control, such as your state file.

Do not commit:

Your terraform.tfstate state file, including terraform.tfstate.* backup state files.
Your .terraform.tfstate.lock.info file. Terraform creates and deletes this file automatically when you run a terraform apply command and contains info about your state lock
Your .terraform directory, where Terraform downloads providers and child modules.
Saved plan files that you create when you include the -out flag when you run terraform plan.
Any .tfvars files that contain sensitive information.
Always commit:

All Terraform code files
Your .terraform.lock.hcl dependency lock file
A .gitignore file that excludes the files listed below
A README.md to describe the code, input variables, and outputs
For an example, refer to GitHub's Terraform .gitignore file.
https://developer.hashicorp.com/terraform/language/style