# Terraform

## Infrasctructure as Code

Infrastructure as code (IaC) is used to automate infrastructure deployment and streamline application development. IaC allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.
There are multiple tools for using IaC to automate resource allocation, here are some examples: HashiCorp Terraform, AWS CloudFormation, Google Cloud Depolyment Manager, Microsoft Azure Resource Manager etc. This [article](https://www.techtarget.com/searchdatacenter/feature/IaC-tools-comparison-shows-benefits-of-automated-deployments) provides a comparison between multiple tools.

## What is Terraform?

Terraform allows you to automate and manage your infrastructure and your platform and services that run on that platform. Infrastructure as Code with Terraform allows you to manage infrastructure with configuration files rather than through a graphical user interface. Terraform is open-source and it uses declarative language, which means you'll only need to define what your end result should be. 

Using Terraform has several advantages over manually managing your infrastructure:

* Terraform can manage infrastructure on multiple cloud platforms.
* The human-readable configuration language helps you write infrastructure code quickly.
* Terraform's state allows you to track resource changes throughout your deployments.
* You can commit your configurations to version control to safely collaborate on infrastructure.

## Terraform workflow:

Terraform generates an execution plan that shows the changes that will be applied to the infrastructure before committing those changes. Terraform can determine what has changed in a configuration and create incremental execution plans that users can apply. 

![Terraform workflow](https://github.com/madetech/data-101/blob/main/images/terraform-workflow.png?raw=true)

1. **Initialize** prepares the working directory so Terraform can run the configuration.
2. **Plan** enables you to preview any changes before you apply them.
3. **Apply** makes the changes defined by your Terraform configuration to create, update, or destroy resources.

- visual elements to see what Terraform is deploying
- explain files (like state file)
- best practices (state file needs to be in a shared space)
- how to reference variables
### Providers
Terraform plugins called providers let Terraform interact with cloud platforms and other services via their application programming interfaces (APIs). HashiCorp and the Terraform community have written over 1,000 providers to manage resources on Amazon Web Services (AWS), Azure, Google Cloud Platform (GCP), Kubernetes, Helm, GitHub, Splunk, and DataDog, just to name a few. 
[Terraform Registry](https://registry.terraform.io/browse/providers).

### Standardise your deployment workflow
Providers define individual units of infrastructure, for example compute instances or private networks, as resources. You can compose resources from different providers into reusable Terraform configurations called modules, and manage them with a consistent language and workflow.

Terraform's configuration language is declarative, meaning that it describes the desired end-state for your infrastructure, in contrast to procedural programming languages that require step-by-step instructions to perform tasks. Terraform providers automatically calculate dependencies between resources to create or destroy them in the correct order.
![Terraform deployment workflow](https://github.com/madetech/data-101/blob/terraform-tutorial/images/terraform.png)


### Track your infrastructure
Terraform keeps track of your real infrastructure in a state file, which acts as a source of truth for your environment. Terraform uses the state file to determine the changes to make to your infrastructure so that it will match your configuration.

### Collaborate
Terraform allows you to collaborate on your infrastructure with its remote state backends. When you use Terraform Cloud (free for up to five users), you can securely share your state with your teammates, provide a stable environment for Terraform to run in, and prevent race conditions when multiple people make configuration changes at once.

You can also connect Terraform Cloud to version control systems (VCSs) like GitHub, GitLab, and others, allowing it to automatically propose infrastructure changes when you commit configuration changes to VCS. This lets you manage changes to your infrastructure through version control, as you would with application code.

## Why Terrraform?
- why is Terraform good for CI/CD?


## Install Terraform on Mac OS
First, install the HashiCorp tap, a repository of all our Homebrew packages.

```
brew tap hashicorp/tap
```

Now, install Terraform with hashicorp/tap/terraform.

```
brew install hashicorp/tap/terraform
```

NOTE: This installs a signed binary and is automatically updated with every new official release.

To update to the latest version of Terraform, first update Homebrew.
```
brew update
```

Then, run the upgrade command to download and use the latest Terraform version.
```
brew upgrade hashicorp/tap/terraform
```

Verify the installation:
```
terraform -help
```

## Quick start tutorial with Docker

[Install Docker for Desktop](https://docs.docker.com/desktop/install/mac-install/)

Now that you've installed Terraform, you can provision an NGINX server in less than a minute using Docker on Mac, Windows, or Linux. You can also follow the rest of this tutorial in your web browser.

After you install Terraform and Docker on your local machine, start Docker Desktop.
```
open -a Docker
```

Create a directory named `learn-terraform-docker-container`.
```
mkdir learn-terraform-docker-container
```
Then, navigate into it.
```
cd learn-terraform-docker-container
```

Paste the following Terraform configuration into a file and name it `main.tf`.
```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 2.13.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

Initialize the project, which downloads a plugin that allows Terraform to interact with Docker.
```
terraform init
```

Provision the NGINX server container with apply. When Terraform asks you to confirm type yes and press ENTER.
```
terraform apply
```

Verify the existence of the NGINX container by visiting `localhost:8000` in your web browser or running docker ps to see the container.

```
docker ps
```

To stop the container, run terraform destroy.
```
terraform destroy
```
You've now provisioned and destroyed an NGINX webserver with Terraform.

## Useful commands in Terraform

![Terraform-cheatsheet](https://github.com/madetech/data-101/blob/terraform-tutorial/images/terraform-cheatsheet-1.png)


### Next:
[Terraform with AWS](Terraform_AWS.md)


### References:
* [Terraform explained in 15 mins (video)](https://www.youtube.com/watch?v=l5k1ai_GBDE)
* [HashiCorp Learn (website)](https://learn.hashicorp.com/terraform)
