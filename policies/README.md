Policy
Policies are rules that HCP Terraform enforces on Terraform runs. You can use policies to validate that the Terraform plan complies with your organization's best practices. For example, you can write policies that:

Limit the size of a web instance
Check for required resource tags
Block deployments on Fridays
Enforce security configuration and cost management
We recommend that you store policies in a separate VCS repository from your Terraform code.

For more information, refer to the policy enforcement documentation, as well as the enforce policy with Sentinel and detect infrastructure drift and enforce policies tutorials.
https://developer.hashicorp.com/terraform/language/style

Govern Terraform
As your teams grow, a common operational challenge is deciding how to enforce your organization's standards and practices. Using codified, automated policy enforcement with Sentinel or OPA ensures consistent application of your standards.

Govern infrastructure through policy
You can use policy as code to ensure your infrastructure meets your organization's security, governance, and cost requirements. You can configure your workflows to automatically run policy checks as part of your Terraform operations and set conditions for how to handle policy failures. Soft enforcement lets prompts a user to approve an operation that fails a policy check, and hard enforcement blocks the operation entirely.

You can define policies that set standards for both your infrastructure configuration itself, and for the workflows around configuration deployment. Some examples of policy rules you can define include which ports are open in a firewall, the permitted sizes of virtual machines, or that deployments cannot take place on Fridays. In HCP Terraform and Terraform Enterprise you can use either OPA or Sentinel for your policy definitions.

Learn how to write a Sentinel policy for a Terraform Deployment and how to detect infrastructure drift and enforce policies.
https://developer.hashicorp.com/terraform/intro/phases/govern