# 1.1 IAC IDE Integration
## Capabilities

This demo covers the following capabilities:

1. Real-time code scanning and feedback

2. One click fixes that resolve security issues for engineers

## Why are they important?

IaC scanning is one piece of the shift security left puzzle. Prisma Cloud's IDE integration enables teams to proactively analyse cloud infrastructure for misconfigurations before it leaves their machines.

Additionally, the contextual documentation and automated fixes the integration provides significantly increases productiviity.

## Scenario

We're starting work on a brand new banking app. At the moment, the Terraform config deploys the following infrastructure:

* A VPC and associated network constructs (routing, subnets, route tables, security groups, etc)

* An Internet Gateway

* An ALB

* An auto-scale group

As our organisation is starting to adopt a "shift left" approach to security, we'd like to use Checkov to scan our code before we deploy it.

## Demo
### Immediate Feedback & Auto Fixes

To ensure we catch issues before code leaves our machines, we're going to start by utilising Prisma Cloud's VSCode integration:

1. Set up the integration by following [(the docs)](https://docs.prismacloud.io/en/classic/appsec-admin-guide/get-started/connect-your-repositories/integrate-ide/connect-vscode)

2. Open VSCode. Click **"File"** --> **"Open Folder"**. Open the `$HOME/banking-app` directory

3. Click on `vpc.tf` and then mouse over the `"subnet_a"` resource. This will show the misconfigurations associated with the resource.

4. Click **"Quick Fix"** and then **"Apply Fix"** to have Checkov resolve the issue for you