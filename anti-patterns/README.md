- circulating TF plans manually with terraform plan -out file option

- premature modularisation / Start simple → abstract only after real reuse patterns emerge

- copy-pasting instead of using modules (drift between copies, maintenance nightmare, no standardization)

- poor module design (leaky modules)

Anti-pattern

Modules that:

expose everything

require 20+ variables

have unclear purpose

Why it’s bad

Hard to use

Hard to maintain

No abstraction

Better

Design modules like APIs:

clear inputs

minimal outputs

opinionated defaults

- using terraform to create cloud accounts/environments

- maintaining manually deployed infrastructure (https://developer.hashicorp.com/well-architected-framework/define-and-automate-processes/process-automation)

- using terraform for configuration management using features like provisioners (terraform is designed for immutable infrastructure operations)

- hard-coding values (not reusable, not environment-aware, painful to scale)

Better

Use variables + environment overlays:

variable "instance_type" {}

And inject via:

tfvars

CI/CD

- not reviewing the plan (yolo apply) by running terraform apply --auto-approve

Why it’s dangerous

Accidental deletions

Security misconfigurations

Production outages

Better

Always review terraform plan

Use approval gates in CD

- running terraform apply locally

Why it’s bad

No audit trail

Inconsistent environments

Credentials leakage risk

“Works on my machine” infra

Better

Run applies via:

CI/CD pipelines

controlled environments

- overusing depends_on 

Why it’s bad

Breaks Terraform’s dependency graph

Slows execution

Hides design issues

Better

Let Terraform infer dependencies via:

references

outputs

- mixing environments in one state
workspace = dev, staging, prod in the same config

Why it’s dangerous

Easy to deploy to wrong environment

Shared state risks

Poor isolation

Better

Separate:

accounts

states

pipelines

- not pinning provider versions
provider "aws" {}

Why it’s risky

Breaking changes

unpredictable behavior

Better:
required_providers {
  aws = {
    version = "~> 5.0"
  }
}

- no testing strategy

Anti-pattern

No validation

No policy checks

No integration tests

Bad because essentially you are doing hope-based infrastructure

Better

Use:

TFLint

Checkov

Terratest

terraform test

- treating terraform as imperative
Anti-pattern mindset:

“Run this, then this, then this…”

Why it’s wrong

Terraform is declarative.

Trying to control execution order leads to:

hacks

null_resource

brittle configs

Better

Describe desired state, not steps.

- Not handling drift

Anti-pattern

Manual changes in AWS:

console edits

hotfixes

Terraform unaware.

Why it’s dangerous

Drift accumulates

future applies break things

Better

Detect drift via terraform plan or terraform plan -refresh-only

enforce “no manual changes” policy

- No Separation of Concerns Between teams

Anti-pattern

Everyone touches everything.

Why it fails

conflicts

ownership confusion

accidental changes

Better

Define ownership:

network team → VPC

platform team → compute

security team → IAM

- Ignoring state design

Anti-pattern

One shared state for everything

No remote backend

No locking

Why it’s dangerous

State corruption

Race conditions

Accidental overwrites

Better

Use:

Remote backends (e.g., S3 + DynamoDB locking)

Separate state per component/environment

What Terraform State Really Is

In Terraform, state is not just a file.

It is:

the source of truth for your infrastructure

a mapping between real resources and code

a locking boundary

a blast radius boundary

So the question becomes:

“How much infrastructure do I want to control (and risk) in a single operation?”

Why a Single Global State Is a Bad Idea

Let’s say you have:

state.tfstate

Containing:

VPC

IAM

RDS

ECS

S3

everything

Problems this creates
1. Massive blast radius

A small mistake:

terraform apply

could:

destroy networking

modify IAM

impact production services

2. State locking bottlenecks

Terraform locks the entire state.

So:

1 engineer running apply → everyone else blocked

terrible for teams

3. Slow plans & applies

Large state = slow diffing

Slower pipelines

Worse developer experience

4. No team ownership boundaries

You can’t cleanly say:

“network team owns this”

“platform team owns that”

Everything is tangled.

5. Higher risk of state corruption

One broken apply = entire infra risk.

3. The Right Mental Model

Think of state as:

A unit of isolation and ownership — not a global database

4. How Mature Teams Design State

They split state based on boundaries.

Pattern 1: By Layer (Most Common)
network-state
compute-state
data-state
security-state

Example:

network → VPC, subnets

compute → ECS, EC2

data → RDS, DynamoDB

Pattern 2: By Environment

dev/network
dev/compute

prod/network
prod/compute
Each environment is isolated.

Pattern 3: By Service (Microservice style)
service-a-state
service-b-state
service-c-state

Used in SaaS/platform setups.

Pattern 4: By Account (Best practice in AWS)
Since you work heavily with AWS, this is key:
account: networking
account: shared-services
account: production
account: staging
Each account has multiple states inside.

- a single repo for all Terraform in a large enterprise is usually an anti-pattern (but so is the practice of having too many repos)

- override files (instead, use variables, modules, and environment-specific configurations for explicit and maintainable overrides)

Avoid using the Terraform -target flag to deploy individual resources within a module, as this breaks dependency tracking and can create inconsistent state. Instead, organize resources into appropriately-sized modules that define your atomic boundaries.