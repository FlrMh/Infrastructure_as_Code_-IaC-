# Terraform
- Terraform is an orchestraction tool used within IaC.
- Terraform is used for building infrastructure, e.g: services (like ec2), vpc's, etc., whereas ansible is used to manage configurations within infrasturcture and services, e.g: provision instances with playbooks.
- Terraform files can b identified by the `.tf`. Terraform scripts are held in the `main.tf` file, located within our local host.

![](pictures/terraform.PNG)

### Install Terraform
[Terraform installation](https://developer.hashicorp.com/terraform/downloads?product_intent=terraform)

[Steps to install Terraform](https://www.youtube.com/watch?v=SkcRSJWNRS8)

- Setting up env variables for our AWS access and secret kets:
1. On your local host, go to the left had search bar and type env. Select the option that is named `Edit environment variables`.
2. Select enviroment variables.
3. Click `New` under `User variables`.
4. Add the `AWS_ACCESS_KEY_ID` and for value, the access key you have been provided.
5. Do the Same for `AWS_SECRET_KEY` and for value, add the secret key you have been provided.
6. Select OK. 
7. Once `terraform` has been installed, open a fresh `Git Bash` terminal, with Admin access, and run:
```
terraform --version
```

## Launching an instance using Terraform

1. 


