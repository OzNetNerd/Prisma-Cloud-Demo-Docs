# 1.3 IAC Pipeline Scanning
## Overview

Demonstrates the following capabilities:

1. IAC & secrets scanning

2. Different settings for different environments:

  * **Dev env:** Fail pipeline on finding **secrets**

  * **Prod env:** Fail pipeline on `HIGH` and `CRITICAL`

## Why are they important?

Our previous demos saw us using [an IDE integration](../1.1._IAC_IDE_Integration/) and a [pre-commit hook](../1.2._IAC_Pre-Commit_Hook/) to save us from pushing insecure code and secrets to git. While this provides enourmous security and productivity benefits, these measures need to be implemented on engineers' machines. As we do not control what tools develoeprs, we need to ensure the scanning takes place in parts of the Software Development Lifecycle (SDLC) that we do control.

## Scenario: Automatically Failing Insecure Changes

In this demo we how Checkov would identify and prevent misconfigurations and secrets from making their way into our applications, **without** having to run anything on our developers' machines.

Currently our environment looks like this:

  1. We have an empty Github repo named `banking-app`

  2. The repo has two branches `dev-banking-app` and `prod-banking-app`

  3. We have a **Github Actions** pipeline configured to run Checkov each time a commit is made to either of these branches. Depending on the branch and the types of findings, Github Actions will perform one of the following actions:

 * `dev-banking-app` will fail the pipeline if there are **secrets** findings
    
 * `prod-banking-app` will fail the pipeline if there are `HIGH` OR `CRITICAL` findings, or **secrets** findings

This decision was made to strike a balance between enabling devs to develop rapidly while enbaling us to start tightening our cloud security.

## Demo
### Push our Terraform to Github

Our cloud infrastructure is written in Terraform, and currently sits on a our machine. We're going to push it to Github so that we can utilise pipeline scanning, collaborate with colleagues, etc.

When deployed, the Terraform config will create the following resourcess:

* A VPC and associated network constructs (routing, subnets, route tables, security groups, etc)

* An Internet Gateway

* An ALB

* An EC2 instance

#### Steps

1. Browse to your [Github repo](https://github.com/<your_username>/banking-app/), then click the **"Compare and pull request"** button. 

2. Select `base: dev-app` and `compare: infra/alpha-0.1`, then click **"Create pull request"**.

  Note: You can watch the Github Actions run by clicking **"Actions"**, then **"Initial Dev infra config"**.

3. Return to the **"Pull reqests"** page once the run finishes. Click **"Merge pull request"**, **"Rebase and merge"**. Next, click **"Confirm rebase and merge"**, then **"Delete branch"**.

### Deploy the code to Dev

Great, our code is now in Github! Next, we need to raise a Pull Request. This enables us to *merge* the updates we made to `infra/alpha-0.1` into `dev-app`.

The merge will automatically trigger GitHub Actions to run. As [mentioned above](#scenario), this will trigger a Checkov scan. What Github Actions does next is determined by the scan results and the destination branch.


### Deploy the code to Prod

1. The errors listed in Github ensure developers don't need to log into Prisma Cloud to understand why their change failed. Other teams can view the results by navigating to "Application Security" --> "Projects" and selecting the repo in the "Repositories" filter.

  Note: This provides limited visibility because the CI/CD pipeline scan only enables us to give a "point in time" view. Next we'll onboard the repo into Prisma Cloud.