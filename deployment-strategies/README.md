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