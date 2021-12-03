_TESTING THIS CHALLENGE WILL CREATE RESOURCES IN YOUR AWS ACCOUNT AND WILL COST A FEW DOLLARS.   
IF THIS IS A PROBLEM FOR YOU, LET US KNOW AND WE WILL PROVIDE YOU WITH AN API KEY TO USE._

# Welcome to the Particle41 Engineering Team Candidate Challenge

This challenge is for candidates who want to join the Particle41 (P41) engineering team ("DevOps").

It is designed to test your level of mastery with common modern development and operations tools and concepts.

## Summary

We aim to hire software engineers who embrace the DevOps mindset, especially taking an infrastructure-as-code approach to software and infrastructure deployment.

This challenge is designed to measure your abilities in the following technologies and concepts:

* Software development (in general), by creating an extremely minimal web service
* Docker, including creating a Docker container image from scratch
* Kubernetes, in particular the AWS cloud-native flavor of Kubernetes (Elastic Kubernetes Service) 
* Terraform, including writing a module and using it to deploy infrastructure.
* Documentation, including a short blurb about the purpose/contents of your repo as well as simple deployment instructions.

This assessment is in two parts, with an extra-credit section at the end.   The first part asks you to create a small application, dockerize it, and then create a kubernetes manifest.    The second part asks you to create a terraform module to create a VPC + EKS cluster, and deploy the application to it.   

---

# Task 1 - Minimalist Application Development / Docker / Kubernetes

## Tiny App Development: 'SimpleTimeService'

- Create a simple microservice (which we will call "SimpleTimeService") in any programming language of your choice: NodeJS, Python, C#, Ruby, whatever you like.  
- The application should be a web server that returns a pure JSON response with the following structure, when its `/` URL path is accessed:

```json
{
  "timestamp": "<current date and time>",
  "ip": "<the IP address of the visitor>"
}
```


## Dockerize SimpleTimeService

- Create a Dockerfile for this microservice.
- Your application must be configured to run as a non-root user in the container.
- Publish the image to a public DockerHub repo so we can pull it for testing.


## Create a k8s manifest for SimpleTimeService

- Create a Kubernetes manifest in YAML format, containing a Deployment and a Service, to deploy your microservice on Kubernetes. 
- Your Deployment must use your public Docker image from DockerHub.

## Push your code to a public git repo

- Push your code to Github, Gitlab, Bitbucket, etc.    MAKE SURE YOU DON'T PUSH ANY SECRETS LIKE API KEYS TO A PUBLIC REPO!
- We have a recommended repo structure [here](##Suggested Repo Structure)


## Acceptance Criteria

Your task will be considered successful if a colleague is able to deploy your manifests to a running Kubernetes cluster and use your microservice.   We will use Docker Desktop to test this exercise.

Assuming that your manifest file is named `microservice.yml`, the command:


```sh
kubectl apply -f microservice.yml # i.e. your manifest file
```

must be the only command needed to deploy your microservice to Kubernetes.

Other criteria for evaluation will be:

- Code quality and style: your code must be easy for others to read, and properly documented when relevant
- Container best practices: your container image should be as small as possible, without unnecessary bloat
- Container best practices: your application should be running as a non-root user, as specified in the exercise


Your task will be considered successful if a colleague is able to deploy your infrastructure to an AWS account, and the infrastructure is created correctly.

Other criteria for evaluation will be:

Code quality and style: your code / Dockerfile / k8s manifest must be technically correct, easy for others to read, and properly documented when relevant

For notes about this, see [Documentation](##Documentation) below


---

# Task 2 - Terraform: Create and Use a Module

## Create my_eks_cluster module

Create a Terraform module called "my_eks_cluster" with the following specs:

- Takes vpc ID and cluster name as input variables (others may be needed - you're the expert, right?)
- Creates EKS cluster using the VPC ID and cluster name provided by variables
- The cluster must have 2 nodes, using instance type `t3a.large`. 
- The nodes must be on the private subnets only.

Of course, you may use popular public registry modules (e.g. the eks module).

## Use my_eks_cluster module to create EKS cluster

Create a root module in terraform that creates the following resources

- A VPC with 2 public and 2 private subnets
- An EKS cluster using the 'my_eks_cluster' module you created in the previous section


As before, you may use popular public registry modules (e.g. the vpc module)

## Push your code to a public git repo

- Push your code to Github, Gitlab, Bitbucket, etc.    MAKE SURE YOU DON'T PUSH ANY SECRETS LIKE API KEYS TO A PUBLIC REPO!
- We have a recommended repo structure [here](##Suggested Repo Structure)

---

## Acceptance Criteria

Your task will be considered successful if a colleague is able to deploy infrastructure to AWS and the correct resources are created.

````sh
terraform plan
````
and
````sh
terraform apply
````

must be the only commands needed to create the EKS cluster.

Other criteria for evaluation will be:

- Code quality and style: your code must be easy for others to read, and properly documented when relevant
- Terraform best practices: don't include API keys or any secrets in the code itself (use environment varibles for the AWS api key)
- Terraform best practices: Use variables in your infrastructure root module, and provide some good defaults in terraform.tfvars

---

# Notes, Suggestions, and the opportunity to 'show off'!

## Documentation

Imagine that someone with less experience than you will need to clone your repository and deploy your k8s container, or deploy your terraform infrastructure. With that in mind, you must provide all the instructions they will need to do that successfully. These must include any prerequisites for deployment; mention of needed tools and links to their installation pages; how to configure credentials for the tool of your choice; and what commands to run for deploying your code.

_We want to see your ability to properly document and communicate about your work with the team._

- Add a `README.md` to the root directory of your project, with instructions for the team to deploy the projects you created.  Include any notes (purpose, etc) that you might leave in the README if this were a real project.
- Publish your code to a public Git repository in a platform of your choice (e.g. GitHub, GitLab, Bitbucket, etc.), so that it can be cloned by the team.


## Suggested Repo Structure

````
.
├── app <-- app files/directories and Dockerfile go here
├── modules
│   └── my_eks_cluster <- module goes here
└── p41-infra
    └── eks <-- root module that consumes module goes here (run 'terraform plan'/'terraform apply' from here)
````

## Extra Credit!

Are you an overachiever? Demonstrate your mastery of cloud-native IaC tooling by doing any of these:

- Additional terraform code to actually deploy the 'simpletimeservice' container image to the EKS cluster
- Additional terraform code to use the Helm provider and any public helm chart to install any standard devops tooling (such as Prometheus)
- Updates to the Kubernetes manifest to utilize best practices (e.g. pod CPU and memory limits)
- A sidecar container of some kind, such as fluentd
- Code to initialize and use the terraform backend (S3 and DynamoDB) for state and locking instead of keeping .tfstate in the repo
- Create a simple CI/CD pipeline (Github Actions, Bitbucket Pipelines, GitLab CI, etc) to publish your Docker container to the DockerHub registry, and commit the config yaml to your solution repo
- Anything else that might demonstrate that you know what's up

