# Prisma Cloud Demos
## Overview

These labs are deployed using AWS, Github and Prisma CLoud APIs. These instructions provide guidance on how to configure the necessary credentials so that Terraform can interact with each of these platforms.

## Instructions

1. Generate Prisma Cloud Access Keys [(docs)](https://docs.prismacloud.io/en/classic/appsec-admin-guide/get-started/generate-access-keys)

2. Install VSCode and the Checkov plugin. Configure the plugin using the Prisma Cloud Access Key [(docs)](https://docs.prismacloud.io/en/classic/appsec-admin-guide/get-started/connect-your-repositories/integrate-ide/connect-vscode)

3. Create an environment variable called `BC_API_KEY` and set its value as the same key as you put into VSCode - e.g `<Access Key>::<Secret Key>`

4. Create a [Github token](https://github.com/settings/tokens) that has `Workflow` and `Delete Repo` permissions

5. Create an environment variable called `GITHUB_TOKEN` and set its value as the Github token

6. Go to the  [Backend Infra](Demos/0._Backend_Infra) directory. Read the README file and deploy the Terraform code.

7. Generate AWS keys for the newly created "github_actions" user. Create the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` and set their values to the "github_actions" keys..

## Backend Architecture

See the  [Backend Infra](Demos/0._Backend_Infra) README for more information on how the backend infrastructure has been architected.

![](Images/BackendInfra.png)

## Demos

* [1. Full SDLC Security](Demos/1._Full_SDLC_Security)

    * [1.1. IAC IDE Integration](Demos/1._Full_SDLC_Security/1.1._IAC_IDE_Integration/)

    * [1.2. IAC Pre-Commit Hook](Demos/1._Full_SDLC_Security/1.2._IAC_Pre-Commit_Hook/)

    * [1.3 IAC Pipeline Scanning](Demos/1._Full_SDLC_Security/1.3._IAC_Pipeline_Scanning/)

**Note:** The [Demo Prep](Demos/0._Backend_Infra/##demo-prep) steps must be completed before the above labs.