# Terraform

## Infrastructure as Code

Infrastructure as code (IaC) is used to automate infrastructure deployment and streamline application development. IaC allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.
There are multiple tools for using IaC to automate resource allocation, here are some examples: HashiCorp Terraform, AWS CloudFormation, Google Cloud Depolyment Manager, Microsoft Azure Resource Manager etc. This [article](https://www.techtarget.com/searchdatacenter/feature/IaC-tools-comparison-shows-benefits-of-automated-deployments) provides a comparison between multiple tools.

## What is Terraform?

Terraform is open-source and allows you to automate and manage your infrastructure and your platform and services that run on that platform. Infrastructure as Code with Terraform allows you to manage infrastructure with configuration files rather than through a graphical user interface.

Using Terraform has several advantages over manually managing your infrastructure:

* Terraform can manage infrastructure on multiple cloud platforms.
* The human-readable configuration language helps you write infrastructure code quickly.
* Terraform's state allows you to track resource changes throughout your deployments.
* You can commit your configurations to version control to safely collaborate on infrastructure.

## Terraform workflow

Terraform generates an execution plan that shows the changes that will be applied to the infrastructure before committing those changes. Terraform can determine what has changed in a configuration and create incremental execution plans that users can apply. 

![Terraform workflow](images/terraform-workflow.png)

To depoly infrastructure with terraform: 

1. **Scope** - identify the infrastructure for your project.
2. **Author** - write the configuration for your infrastructure.
3. **Initialize** - prepares the working directory so Terraform can run the configuration.
4. **Plan** - enables you to preview any changes before you apply them.
5. **Apply** - makes the changes defined by your Terraform configuration to create, update, or destroy resources.

### Important concepts

Terraform plugins called **providers** let Terraform interact with cloud platforms and other services via their application programming interfaces (APIs). Terraform has over 1,000 providers to manage resources on Amazon Web Services (AWS), Azure, Google Cloud Platform (GCP) etc. 

Terraform has a modular approach, which means it keeps it's configuration into reusable **modules** managing them with a consistent language and workflow, for less duplication. 

Terraform uses **declarative language**, which means you'll only need to define what your end result should be and terraform providers will automatically calculate dependencies between resources to create or destroy them in the correct order.

Terraform keeps track of your real infrastructure in a **state file**, which acts as a source of truth for your environment. Terraform uses the state file to determine the changes to make to your infrastructure so that it will match your configuration. By default this is stored in a local file named `terraform.tfstate`, but it is best practice to store this remotely, especially in a team environment. This file is automatically updated to the state of the real infrastructure.

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

![Terraform-cheatsheet](images/terraform-cheatsheet.png)


### References:
* [Terraform explained in 15 mins (video)](https://www.youtube.com/watch?v=l5k1ai_GBDE)
* [HashiCorp Learn (website)](https://learn.hashicorp.com/terraform)
