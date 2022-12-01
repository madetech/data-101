# Terraform
## What is Terraform
Terraform allows you to automate and manage your infrastructure and your platform and services that run on that platform. 

Infrastructure as Code with Terraform allows you to manage infrastructure with configuration files rather than through a graphical user interface.IaC allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.

Using Terraform has several advantages over manually managing your infrastructure:

* Terraform can manage infrastructure on multiple cloud platforms.
* The human-readable configuration language helps you write infrastructure code quickly.
* Terraform's state allows you to track resource changes throughout your deployments.
* You can commit your configurations to version control to safely collaborate on infrastructure.

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

## Terraform workflow:
1. **Initialize** prepares the working directory so Terraform can run the configuration.
2. **Plan** enables you to preview any changes before you apply them.
3. **Apply** makes the changes defined by your Terraform configuration to create, update, or destroy resources.

- visual elements to see what Terraform is deploying
- explain files (like state file)
- best practices (state file needs to be in a shared space)
- how to reference variables
- what is a provider?
Terraform plugins called providers let Terraform interact with cloud platforms and other services via their application programming interfaces (APIs). HashiCorp and the Terraform community have written over 1,000 providers to manage resources on Amazon Web Services (AWS), Azure, Google Cloud Platform (GCP), Kubernetes, Helm, GitHub, Splunk, and DataDog, just to name a few. Find providers for many of the platforms and services you already use in the [Terraform Registry](https://registry.terraform.io/browse/providers). If you don't find the provider you're looking for, you can write your own.
Providers are a logical abstraction of an upstream API. They are responsible for understanding API interactions and exposing resources.


![Terraform deployment workflow](images/terraform.png)

## Why Terrraform?
- why is Terraform good for CI/CD?


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


### Next:
[Terraform with AWS](Terraform_AWS.md)


### References:
* [Terraform explained in 15 mins (video)](https://www.youtube.com/watch?v=l5k1ai_GBDE)
* [HashiCorp Learn (website)](https://learn.hashicorp.com/terraform)
