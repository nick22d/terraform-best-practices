
- install only necessary providers
- do not embed credentials / store them outside terraform (i.e. vault)
- do not version control any sensitive information


---
protect sensitive data

Terraform provides two tools for resources to manage data you do not want to store in state or plan files: the ephemeral resource block and ephemeral write-only arguments on specific resources.

Use write-only arguments to securely pass temporary values to resources during a Terraform operation without worrying about Terraform persisting those values. 
https://developer.hashicorp.com/terraform/language/manage-sensitive-data/ephemeral

- sensitive outputs
- sensitive variables
- ephemeral arguments and blocks to omit sensitive data from state and plan files entirely
- Write-only arguments let you securely pass temporary values to Terraform's managed resources during an operation without persisting those values to state or plan files. 

it is possible to combine sensitive and ephemeral arguments in outputs and variables 

https://developer.hashicorp.com/terraform/language/manage-sensitive-data

we recommend using write-only arguments for passing ephemeral values to resources. For example, you can use an ephemeral resource to generate a random password and pass it to the password_wo write-only argument:
https://developer.hashicorp.com/terraform/language/manage-sensitive-data/write-only

