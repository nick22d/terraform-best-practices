Each monitoring agent has specific installation and configuration methods, but there are common best practices that apply to them.

Securely manage monitoring agents secrets
Configure and deploy your monitoring agent on VMs
Configure monitoring agent on container orchestrators (Kubernetes and Nomad)
Platform agnostic monitoring through service mesh
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/monitor/setup-monitoring-agents

We recommend that you retrieve the required credentials from a secure secret management solution like HashiCorp Vault during the deployment process.

If you use Packer, you can directly pull dynamic secrets from Vault when you build the golden image.
If you use configuration management tools like Ansible, you can integrate it with Vault to securely inject monitoring credentials when you configure the monitoring agent.
When you deploy your image with Terraform, you can leverage user scripts (startup/cloud-init scripts) to fetch the credentials from Vault.
This secret injection approach keeps credentials out of your codebase even as you adopt “as-code” best practices to configure your monitoring agents. Vault lets you securely store and distribute secrets through strict access controls and auditing capabilities.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/monitor/setup-monitoring-agents/manage-secrets

You can configure and deploy monitoring agents in different ways:

Use golden images with the monitoring agent already configured. (Recommended)
Use a post-provisioning script or configuration management tools to configure the monitoring agent.
We recommend using golden images that include the pre-installed monitoring agents, which developers can use to build their applications. These produce faster, more consistent deployments and reduce the risk of flawed deployments.

Post-provisioning workflows require more time and effort to install and configure software on each individual system after provisioning, which can lead to inconsistencies and increased maintenance overhead.

We recommend you use Terraform to deploy and manage your virtual machines. Terraform can deploy virtual machines from golden images created by Packer or execute configuration management scripts to install and configure agents. Adopting infrastructure-as-code practices with Terraform provides a consistent, versionable workflow for your infrastructure.

How you deploy monitoring agents depends on how you configure and install them on virtual machines. The following sections provide resources for deploying pre-built images created with Packer, as well as deploying virtual machines and installing agents through post-provisioning scripts.

We recommend creating a base machine image that already has your chosen monitoring agent installed. This ensures consistency, since each service will automatically send monitoring data to your central monitoring platform. Your organization can use Packer with configuration management tools, like Ansible, to create consistent and centrally-managed images. These preconfigured “golden images” have all the necessary software, dependencies, and security patches to run your services.

If your organization or environment prohibits you from using golden image pipelines you can automatically install monitoring agents after you create the virtual machine (VM).

For example, you can write a script that installs the monitoring agent and configures it to send data to your central monitoring platform. When you use a tool like Terraform to deploy the VM, you can tell it to run this script after the VM launches. Each cloud provider has a different method to run these scripts. In AWS for example, you can provide this script to the aws_instance resource with the user_data argument.

Another option is to use configuration management tools like Chef, Puppet, or Ansible. These tools provide a structured way to consistently configure software across your services.

These post-provisioning methods may take longer to set up the monitoring agents and services after creating the VM than using a machine image. The monitoring agent configuration can become inconsistent if the script or configuration has errors. You also incur the maintenance burden of managing the script lifecycle to support more services as your infrastructure changes.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/monitor/setup-monitoring-agents/vm

As the number of services you manage and maintain grows, manually managing monitoring components like dashboards and alerts become unsustainable. This can lead to inconsistencies across environments, observability gaps where issues go undetected, and potential security risks. We recommend adopting monitoring-as-code (MaC) to manage these configurations to solve these challenges as your organization scales.

With monitoring-as-code (MaC), you can adopt many of the best practices as infrastructure-as-code (IaC), such as:

Consistent configuration: MaC lets you consistently deploy standardized monitoring setups across teams and environments. Terraform lets you create modules that include standard monitoring configurations with built-in reasonable defaults. A range of monitoring tools also offer official Terraform providers and modules your organization can use.

For example, the team responsible for ensuring data integrity can make changes to the Terraform modules and propagate those changes throughout the organization.

Automated provisioning: As your organization scales, you can configure infrastructure and service deployments to automatically trigger monitoring dashboards.

Auditable changes: All changes to monitoring components are traceable through version control.

Policy-compliant resources: You can use Sentinel and Open Policy Agent (OPA) to ensure your monitoring resources are secure and compliant with your organization's policies.

While the codified approach offers significant benefits, designing complex monitoring dashboards and alert rules directly in code can be challenging initially.

To balance this, we recommend an iterative workflow. First, leverage your monitoring tool's UI to visually design and build dashboards, layouts, and alert rules. This allows you to fully utilize the robust querying capabilities and intuitive interfaces provided by monitoring solutions. Once you have functional prototypes, import those configurations into Terraform code and standardize them as a Terraform module. From there, your organization can consume and modify the Terraform modules to create consistent monitoring dashboards and alerts across your infrastructure and services.

This approach combines the flexibility of visually designing your dashboards first with the consistency and maintainability of managing it as code.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/monitor/dashboards-alerts