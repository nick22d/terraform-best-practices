Multiple environments
We recommend that your repository's main branch be the source of truth for all environments. For HCP Terraform and Terraform Enterprise users, we recommend that you use separate workspaces for each environment. For larger codebases, we recommend that you split your resources across multiple workspaces to prevent large state files and limit unintended consequences from changes. For example, you could structure your code as follows:

.
├── compute
│   ├── main.tf
│   ├── outputs.tf
│   └── variables.tf
├── database
│   ├── main.tf
│   ├── outputs.tf
│   └── variables.tf
└── networking
    ├── main.tf
    ├── outputs.tf
    └── variables.tf

In this scenario, you would create three workspaces per environment. For example, your production environment would have a prod-compute, prod-database, and prod-networking workspace. Read more about Terraform workspace and project best practices.

If you do not use HCP Terraform or Terraform Enterprise, we recommend that you use modules to encapsulate your configuration, and use a directory for each environment so that each one has a separate state file. The configuration in each of these directories would call the local modules, each with parameters specific to their environment. This also lets you maintain separate variable and backend configurations for each environment.

├── modules
│   ├── compute
│   │   └── main.tf
│   ├── database
│   │   └── main.tf
│   └── network
│       └── main.tf
├── dev
│   ├── backend.tf
│   ├── main.tf
│   └── variables.tf
├── prod
│   ├── backend.tf
│   ├── main.tf
│   └── variables.tf
└── staging
    ├── backend.tf
    ├── main.tf
    └── variables.tf

https://developer.hashicorp.com/terraform/language/style