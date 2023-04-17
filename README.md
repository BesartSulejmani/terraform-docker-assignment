# Terraform Docker Assignment

This assignment is used to test your terraform skills. You're going to be using the following skills learned over the course to deploy docker containers:
- providers
- resources
- data sources
- variables & inputs
- modules
- dynamic blocks

## Pre-requisites

**Set up Windows Subsystem for Linux 2 (WSL2):**

https://learn.microsoft.com/en-gb/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package

**Install Docker desktop:**

After installing set the following setting(s) for Windows
- Expose daemon on tcp://localhost:2375 without TLS
- Use the WSL 2 based engine

## Assignment 1: deploy basic ubuntu image

1. Set up the provider "kreuzwerker/docker"
2. Set the provider version to 3.0.2
3. To connect to docker desktop you need to declare a host in your provider block:
   1. Windows: host = "tcp://localhost:2375"
   2. MacOS: host = "unix:///var/run/docker.sock"
4. Create a resource referencing the latest docker image for ubuntu
5. Create a docker container that references the image above to deploy a container

Use variables if you want (not necessary for this assignment)
   
```
TIP: Use the documentation: https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs
```

## Assignment 2: deploy nginx container as a module

Finish the module for the nginx container

1. Create a docker network resource with only the required attribute(s) and use variables
2. Create a docker volume resource with only the required attribute(s) and use variables
3. Create a docker image refering the latest nginx tag

4. Create a docker container using the image above as a reference
   1. Set the container name as a variable
   2. Set up a dynamic block for the sub-resource "ports" containg only the internal and external attributes with for_each = var.ports == null ? [] : var.ports
   3. Set up the networks_advanced sub-resource refering to the docker network resource
   4. Set up the upload sub-resource containing the file and source attributes. Source must be declared as a variable.
   5. Set up the volumes sub-resource and reference the volume_name, host_path and container_path attributes. The first two reference the docker volume and the container path is set to "/usr/share/nginx/html/"

5. Create outputs for your module
   1. Set an output for volumes refering to the volumes value
   2. Set an output for port refering to the publish_all_ports value
   3. Set an output for the docker network refering to the id of the network

6. Set up the following variables at the least:
   1. ports as a type of list(object) with the default null
   2. source for the upload sub-resource to upload a file of the type string

Fill the rest in as you wish like container name, image name etc.

7. Set the same provider in the providers.tf created earlier otherwise terraform will try to reach the hashicorp/docker provider which doesn't exist. The source is kreuzwerker/docker.

8. Back in your main.tf outside of the module, declare the module to be created and fill in the parameters needed for the module (variables in your module)

9. Set up a datasource using the "local_file" type to pass through the index.html file with only the required attribute: https://registry.terraform.io/providers/hashicorp/local/latest/docs/data-sources/file

10. For your source parameter declared in the module, reference the datasource above.

11. For the ports parameter, set the same variables in the variables.tf as you did in your module.

12. Set the value for the ports variable in the terraform.tfvars file. Internal port = 80 and external port = 8000

13. Your module will create a docker network. Add a networks_advanced sub-resource to the ubuntu container that you created in assignment 1 and reference to the output that you declared in the module.