Workflow style
This section reviews standards that enable predictable and secure Terraform workflows, such as:

Pin your Terraform, provider, and module versions.
Name your module repositories using this three-part name terraform-<PROVIDER>-<NAME> when using the HCP Terraform registry.
Store local modules at ./modules/<module_name>.
Use the tfe_outputs data source or provider-specific data sources to share state between two state files.
Protect credentials by using dynamic provider credentials or a secrets manager such as HashiCorp Vault.
Write tests for your modules.
Use policy enforcement on HCP Terraform to set guardrails for infrastructure operations.
https://developer.hashicorp.com/terraform/language/style

GitOps workflow
GitOps is a deployment methodology that uses Git repositories as the single source of truth for both application code and infrastructure configuration. When developers want to make changes, they submit pull requests to update the Git repository. Once merged, the GitOps tooling automatically applies those changes to the target environment. The GitOps development cycle creates a fully auditable, version-controlled deployment process, where Git serves as the control plane for your entire infrastructure and application lifecycle.

Consistency: By using Git as the source of truth, you ensure GitOps deploys your infrastructure and applications consistently across environments, with development, staging, and production all managed through the same GitOps workflow.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/process-automation/gitops

Implement the following testing practices in your deployment workflow:

Container image scanning: Scan Docker images for security vulnerabilities using security scanning tools before pushing to your registry. Fail the build if critical vulnerabilities are found.

Image validation tests: Verify that packaged images start correctly and contain expected files and dependencies. Test that your application responds to requests and passes health checks.

Deployment smoke tests: After deploying with Terraform, run automated tests that validate your application is accessible, health endpoints respond correctly, and critical functionality works.

Progressive deployment testing: Use blue-green or canary deployments to test new versions in production with a small percentage of traffic before full rollout. Monitor error rates and performance metrics during the deployment.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/automate/testing
