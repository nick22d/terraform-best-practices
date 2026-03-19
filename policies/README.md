Policy
Policies are rules that HCP Terraform enforces on Terraform runs. You can use policies to validate that the Terraform plan complies with your organization's best practices. For example, you can write policies that:

Limit the size of a web instance
Check for required resource tags
Block deployments on Fridays
Enforce security configuration and cost management
We recommend that you store policies in a separate VCS repository from your Terraform code.

For more information, refer to the policy enforcement documentation, as well as the enforce policy with Sentinel and detect infrastructure drift and enforce policies tutorials.
https://developer.hashicorp.com/terraform/language/style