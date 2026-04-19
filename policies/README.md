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

nforce policies with Sentinel
Fully-automated deployments require automated validation to prevent dangerous changes from reaching production. Manual code reviews and approval gates create bottlenecks that slow deployments. When operations teams manually review every infrastructure change, they become deployment bottlenecks. However, removing manual reviews without automated checks allows dangerous changes—like publicly exposing databases, deploying oversized instances, or removing security groups—to reach production.

Sentinel provides policy as code for HCP Terraform, automatically validating infrastructure changes before deployment. You define policies that enforce security requirements, cost controls, and compliance standards. HCP Terraform evaluates these policies during the plan phase and blocks changes that violate policies, allowing safe changes to proceed automatically.

The following example shows Sentinel policies that enforce security and cost controls:

# Policy enforcement for automated deployments
import "tfplan/v2" as tfplan

# Require all S3 buckets to be private
mandatory_s3_encryption = rule {
  all tfplan.resource_changes as _, rc {
    rc.type is "aws_s3_bucket" implies
      rc.change.after.acl is "private" and
      rc.change.after.versioning[0].enabled is true
  }
}

# Limit EC2 instance sizes to control costs
restrict_instance_types = rule {
  all tfplan.resource_changes as _, rc {
    rc.type is "aws_instance" implies
      rc.change.after.instance_type in ["t3.micro", "t3.small", "t3.medium"]
  }
}

# Require tags for resource management
require_resource_tags = rule {
  all tfplan.resource_changes as _, rc {
    rc.type is "aws_instance" implies (
      "Environment" in keys(rc.change.after.tags) and
      "Owner" in keys(rc.change.after.tags)
    )
  }
}

main = rule {
  mandatory_s3_encryption and
  restrict_instance_types and
  require_resource_tags
}

The Sentinel policies automatically validate every Terraform plan in your CI/CD pipeline. When a developer tries to create a public S3 bucket, deploy an oversized instance, or skip required tags, Sentinel blocks the change and explains the violation. Approved changes that meet all policies proceed automatically to deployment without manual review. Policy automation enables rapid deployments while maintaining security and compliance standards. HCP Terraform enforces these policies before applying changes, preventing policy violations from reaching your infrastructure.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/process-automation/fully-automated

Sentinel is HashiCorp's policy-as-code framework that validates infrastructure configurations before Terraform applies them. Use Sentinel with HCP Terraform to enforce security policies, compliance requirements, and organizational standards across all infrastructure changes. Sentinel policies evaluate Terraform plans and block non-compliant changes before Terraform applies them.

Use Sentinel when you need to enforce policies across teams, ensure security requirements are met automatically, or validate that infrastructure configurations comply with organizational standards before deployment.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/automate/testing

HCP Terraform uses Sentinel to enable granular policy control for your infrastructure. Sentinel is a language and policy framework, which restricts Terraform actions to defined, allowed behaviors. Policy authors manage Sentinel policies in HCP Terraform with policy sets, which are groups of policies. Organization owners control the scope of policy sets by applying certain policy sets to the entire organization or by selecting workspaces.
https://developer.hashicorp.com/well-architected-framework/secure-systems/compliance-and-governance/policy-as-code