# Particle41 DevOps Challange - Senior level

This is a senior-level challenge for candidates who want to join the Particle41 DevOps team.

It is designed to test your level of familiarity with development and operations tools and concepts.

You will have **4 hours** to complete the challenge.


## The challenge

You must use either Terraform, AWS CloudFormation, or Pulumi for all the following tasks. You can use modules from the Terraform registry.

- Create code for deploying a VPC in AWS with 2 public and 2 private subnets.

- Create code for deploying an EKS cluster in AWS, which will use the VPC created in the previous step. The cluster must have 2 nodes, using instance type `t3a.large`. The nodes must be on the private subnets only.

- Add a `README.md` to the root directory of your project, with instructions for the team to deploy the infrastructure you created.

- Publish your code to a public Git repository in a platform of your choice (e.g. GitHub, GitLab, Bitbucket, etc.), so that it can be cloned by the team.


## Criteria


Your task will be considered successful if a colleague is able to deploy your infrastructure to an AWS account.

Other criteria for evaluation will be:

- Code quality and style: your code must be easy for others to read, and properly documented when relevant
