
- install only necessary providers
- do not embed credentials / store them outside terraform (i.e. vault)
- do not version control any sensitive information


---
protect sensitive data

- sensitive outputs
- sensitive variables
- ephemeral arguments and blocks to omit sensitive data from state and plan files entirely
- Write-only arguments let you securely pass temporary values to Terraform's managed resources during an operation without persisting those values to state or plan files. 

it is possible to combine sensitive and ephemeral arguments in outputs and variables 

https://developer.hashicorp.com/terraform/language/manage-sensitive-data