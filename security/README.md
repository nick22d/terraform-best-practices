
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

Terraform evaluates the command in a local shell and can use environment variables for variable substitution. We do not recommend using Terraform variables for variable substitution because doing so can lead to shell injection vulnerabilities. Instead, you should pass Terraform variables to a command through the environment parameter and use environment variable substitution instead. Refer to the following OWASP article for more information about injection flaws: Code Injection.
https://developer.hashicorp.com/terraform/language/block/removed

Use secrets storage
Your configuration may rely on sensitive values, such as provider credentials. Although you can mark certain variables as sensitive to prevent displaying them as plaintext in run output, a more robust solution is to use secrets storage such as HashiCorp Vault

Vault securely stores sensitive information such as credentials and provides granular access control. You can integrate Vault into your Terraform configuration using the Vault provider. If you deploy your infrastructure to a major cloud provider, such as AWS, you can also generate short-lived credentials with Vault or use dynamic provider credentials, which prevents having to store credentials.

Vault also integrates into many popular CI/CD solutions such as GitHub, Jenkins, and CircleCI. Vault provides a central system to store and access data, which lets CI/CD pipelines push and pull secrets programmatically.
https://developer.hashicorp.com/terraform/intro/phases/adopt

Establish a centralized logging infrastructure to collect audit data from all systems. Use log aggregation tools to normalize different log formats and create a unified view of system activity. Implement log retention policies that align with your compliance requirements and legal obligations.

Create automated alerting for suspicious audit events. Set up monitoring for unusual access patterns, failed authentication attempts, or unauthorized configuration changes. Use log analysis tools to correlate events across different systems and identify potential security threats.
https://developer.hashicorp.com/well-architected-framework/secure-systems/compliance-and-governance/audit-trails