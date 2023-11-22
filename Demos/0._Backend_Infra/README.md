# Backend Infra

While Github Actions is responsible for executing code & configuration changes, it uses ephemeral storage and therefore cannot be used to store Terraform Statefiles. As illustrated below:

1. The **"Backend Infra"** Terraform is run a SA’s machine and therefore its Statefile will be stored locally

2. The config creates the following resources:
  * An S3 bucket

  * An AWS user that will be used by Github Actions

  * An IAM policy that allows the AWS user to reach and write to the S3 bucket

3. The SA will create AWS credentials for the aforementioned AWS user and will configure them in Github Actions as instructed in the demos. 

Each time a new commit is pushed into one of the demo repos, Github Actions will use the AWS keys to read the Terraform statefile, make the necessary changes, and then update the statefile. Additionally, Github Actions will use these credentials to deploy (and destroy) the AWS infrastructure specified in each demo's Terraform config.

**Note:** This means that the Github user’s policy needs to have enough permissions to create, update and delete all of the resources in the demo repos.

![](../Images/BackendInfra.png)