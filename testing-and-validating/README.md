Postconditions can serve as static guardrails to enforce mandatory configuration aspects on your data and resource blocks. For verifying infrastructure dynamically against external or changing conditions, we recommend using the check blocks, which run as the final step of a Terraform operation after postconditions.

To decide between a precondition or a postcondition, consider whether the rule you are setting represents an assumption you need to make about the configuration, or a guarantee on the resulting resource, and when it should run.

Use preconditions for assumptions that you want to verify before Terraform creates the target block. For example, you may want to verify that the AMI selected for an aws_instance has x86_64 CPU architecture before trying to create the instance. Using preconditions for assumptions helps future maintainers understand the values a resource, output, or data source should allow.

Use postconditions for guarantees that you need to verify after Terraform creates the resource or reads from the data source. For example, you may want to ensure that an aws_instance is launched in a network that assigns it a private DNS record. Use postconditions for guarantees to help future maintainers understand which behaviors they must preserve when changing configuration.

We recommend writing error messages as one or more full sentences in a style similar to Terraform's own error messages. Terraform shows the message alongside the name of the resource that detected the problem and any external values included in the condition expression.


terratest
https://developer.hashicorp.com/terraform/language/validate

check blocks

https://developer.hashicorp.com/terraform/language/block/check

Actions are preset operations built into providers that let Terraform perform day-two operations. Invoke actions to trigger automations outside of Terraform, such as Ansible playbooks and Lambda jobs. Actions do not affect resource state.
https://developer.hashicorp.com/terraform/language/invoke-actions

You may need to upload files, run commands and scripts, and perform other operations to prepare resources you create and manage with Terraform for service. Terraform is primarily designed for immutable infrastructure operations, so we strongly recommend using purpose-built solutions to perform post-apply operations.
https://developer.hashicorp.com/terraform/language/post-apply-operations