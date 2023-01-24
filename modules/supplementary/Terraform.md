# Terraform
## What is Terraform
Terraform allows you to automate and manage your infrastructure and your platform and services that run on that platform. 

Infrastructure as Code with Terraform allows you to manage infrastructure with configuration files rather than through a graphical user interface.
IaC allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.

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

# Name of directory
## The Scenario
This code extends the tutorial found at [Hashicorp](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/aws-get-started), which deploys a docker container called 'Tutorial' running an nginx instance, by mounting a volume to load custom HTML content and an image.

## Pre-requisites
- Terraform (v12.0+) - this can be found by going to the [hashicorp](https://github.com/ned1313/Getting-Started-Terraform) website. 
- Docker Desktop (Or at least Docker installed and configured locally) - instructions on installing this and setting up can be found at the [Docker website](https://docs.docker.com/get-docker/).
- Populating a tfvars file as explained in the variables section.

## Content overview
---If you add any content on commit, please update the README below accordindly---
Please use the headers below to list:
- ![#808080](https://via.placeholder.com/15/808080/000000?text=+) `Directory Structure - file layout with brief descriptions of contents`
- ![#800080](https://via.placeholder.com/15/800080/000000?text=+) `Terraform Functions - List any featuring in configuration`
- ![#000000](https://via.placeholder.com/15/000000/000000?text=+) `Configuration contents - Variables, Providers, Locals, Modules, Data, Resources and Output information`

## How to use this configuration

### Terraform commands sequence
1) `terraform init` - Terraform requires initialisation at the command level.
2) `terraform validate` - This will check to make sure you're configuration is valid.
3) `terraform plan -out docker<name>.tfplan` - This will create a plan of proposed deployments, feel free to check the output. (example name: dockerjohn.tfplan)
4) `terraform apply "docker<name>.tfplan"` - This will apply the plan proposed above and actually build your infrastructure. (example name: dockerjohn.tfplan)
5) (OPTIONAL) `docker ps -f name=tutorial --format 'table {{.Names}}\t{{.Status}}'` - This command will confirm if the container has been created and how long for
6) `terraform destroy` - When you are done, you can use this to delete everything you created

### Successful deployment conditions
The console will output a URL, which can be retrieved again with `terraform output`.
You will know if you successfully deployed it when you can reach the HTML within this directory, being run by an nginx instance. This will be true across all container instances.

### Directory Structure
```diff
# html/index.html <- the Html file to be loaded>
# html/Companies_House_logo.png <- An image to accompany the html file>
# README.md <- What you're reading now, please update if you make changes>
# main.tf <- The configuration >
# terraform.tfvars.example
```

### Terraform functions
These are specific functions that the terraform language understands, and have appropriate documentation on the [terraform functions website](https://www.terraform.io/docs/language/functions)
```diff
@@ .rendered - used in data.template_file.ports[*].rendered which is a special terraform attribute to return a list from a template file. Its the primary way to get around the lack of count support in outputs @@
```

### Configuration contents

#### Variables
You can set these within a terraform.tfvars file (see the example provided). Please do not remove or change the default values (as it wouldn't adhere to our terraform standards)
* host_path - String - REQUIRED - the path in your local directory, sourced from a tfvars file, where you want content to be loaded from.
* external_port_prefix - Number - OPTIONAL - the port attached to localhost for the nginx front end, defaulted at 800
* docker_container_name - String - OPTIONAL - whatever name you want to call the container, as a String
* instance_count - Number - OPTIONAL - the number of containers to spin up

#### Providers
* docker

#### Locals
None

#### Data
* ports <- This is to specify all the ports that will be used for the containers. Necessary for dns_entries to return a list>

#### Modules
None

#### Resources
* docker_image <- This is to specify which Docker Image we are to build from and is used in the next resource>
* docker_container <- This is the container itself and it's configuration. It has volume mounting enabled>

#### Outputs
* dns_entries <- A list of the DNS addresses needed, dynamically generated with a special terraform function>
* docker_process_checker <- A useful command line function to ensure your container has been built>

### Next:
[Terraform with AWS](Terraform_AWS.md)


### References:
* [Terraform explained in 15 mins (video)](https://www.youtube.com/watch?v=l5k1ai_GBDE)
* [HashiCorp Learn (website)](https://learn.hashicorp.com/terraform)
