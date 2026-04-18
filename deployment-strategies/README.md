blue-green
canary
A/B testing
rolling


Promote through environments: Validate automation in non-production before promoting to production, catching issues early and reducing production incidents. Start with a development environment, progress to staging, then deploy to production with confidence.

Implement progressive deployments: Deploy changes gradually with zero-downtime strategies like blue-green or canary deployments, minimizing risk and enabling quick rollbacks when issues occur.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/automate/deployments

Atomic deployments deploy infrastructure as small, isolated units of change rather than deploying all infrastructure components together. Large, monolithic deployments that modify multiple systems simultaneously create risk, make troubleshooting difficult, and slow down rollback operations when issues occur. Deploy infrastructure atomically to reduce blast radius, enable independent team workflows, and accelerate your deployment velocity.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/deploy/atomic-deployments

Integrate atomic deployments into CI/CD pipelines to automate testing and deployment of individual infrastructure components. Configure pipelines to detect changes in specific module directories and deploy only the affected modules, reducing deployment time and risk.

The following example shows a GitHub Actions workflow that deploys atomic modules based on changed files:

name: Deploy Infrastructure

on:
  push:
    branches: [main]

jobs:
  detect-changes:
    runs-on: ubuntu-latest
    outputs:
      networking: ${{ steps.changes.outputs.networking }}
      database: ${{ steps.changes.outputs.database }}
      application: ${{ steps.changes.outputs.application }}
    steps:
      - uses: actions/checkout@v3
      - uses: dorny/paths-filter@v2
        id: changes
        with:
          filters: |
            networking:
              - 'infrastructure/networking/**'
            database:
              - 'infrastructure/database/**'
            application:
              - 'infrastructure/application/**'

  deploy-networking:
    needs: detect-changes
    if: needs.detect-changes.outputs.networking == 'true'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy networking
        run: |
          cd infrastructure/networking
          terraform init
          terraform plan
          terraform apply -auto-approve

  deploy-database:
    needs: [detect-changes, deploy-networking]
    if: needs.detect-changes.outputs.database == 'true'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy database
        run: |
          cd infrastructure/database
          terraform init
          terraform plan
          terraform apply -auto-approve

  deploy-application:
    needs: [detect-changes, deploy-networking, deploy-database]
    if: needs.detect-changes.outputs.application == 'true'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy application
        run: |
          cd infrastructure/application
          terraform init
          terraform plan
          terraform apply -auto-approve

The GitHub Actions workflow detects which modules changed and deploys only the affected components. If only application code changes, the pipeline deploys only the application module, skipping networking and database. Deploying only changed modules reduces deployment time and limits the scope of changes in each deployment.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/deploy/atomic-deployments

when to use each deployment strategy
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/deploy/zero-downtime-deployments

Deploy your green infrastructure environment only when needed, test it thoroughly, then switch traffic and tear down the blue environment to reduce costs.

Use Terraform modules to deploy identical infrastructure for blue and green environments. Create a reusable module that defines your infrastructure, then instantiate it twice with different environment variables.

The following example shows a Terraform module structure for blue/green infrastructure:

variable "environment" {
  description = "Environment identifier (blue or green)"
  type        = string
}

resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type

  tags = {
    Name        = "web-server-${var.environment}"
    Environment = var.environment
  }
}

resource "aws_lb_target_group" "web" {
  name     = "web-targets-${var.environment}"
  port     = 80
  protocol = "HTTP"
  vpc_id   = var.vpc_id

  health_check {
    enabled           = true
    healthy_threshold = 2
    path              = "/health"
  }
}

output "target_group_arn" {
  value = aws_lb_target_group.web.arn
}

# Instantiate blue and green environments
module "blue_infrastructure" {
  source = "./modules/infrastructure"

  environment   = "blue"
  instance_type = "t3.micro"
  ami_id        = var.blue_ami_id
  vpc_id        = aws_vpc.main.id
}

module "green_infrastructure" {
  source = "./modules/infrastructure"

  environment   = "green"
  instance_type = "t3.micro"
  ami_id        = var.green_ami_id
  vpc_id        = aws_vpc.main.id
}

# Switch traffic using variable
variable "active_environment" {
  description = "Active environment (blue or green)"
  type        = string
  default     = "blue"
}

resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.main.arn
  port              = "80"
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = var.active_environment == "blue" ? module.blue_infrastructure.target_group_arn : module.green_infrastructure.target_group_arn
  }
}

The Terraform configuration creates two identical infrastructure environments with different AMI IDs. The load balancer routes traffic based on the active_environment variable. To switch from blue to green, update active_environment = "green" and run terraform apply. The load balancer immediately routes traffic to the green environment, achieving zero-downtime deployment.
https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/deploy/zero-downtime-deployments/infrastructure