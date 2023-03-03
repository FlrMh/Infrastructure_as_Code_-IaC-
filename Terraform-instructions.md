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

![](pictures/env-var-system.PNG)

![](pictures/create-env-var.PNG)

### For security purposes, from this point on, I will not be able to supply anymore screenshots as my Environment Variables will be exposed.
3. Click `New` under `User variables`.

4. Add the `AWS_ACCESS_KEY_ID` as the title of the Environment Variable, and for value, the AWS access key you have been provided. select OK when you are sure that the Environmental Variable is correct.
5. Do the same for `AWS_SECRET_KEY`, using for value the AWS secret key you have been provided.
6. Select OK. 
7. Once `Terraform` has been installed, open a fresh `Git Bash` terminal, with Admin access, and run:
```
terraform --version
```
- Once terraform is up and running, we are ready to try and launch an `EC2` instance.


## Launching an instance using Terraform

1. In order to launch an `Ec2` instance using terraform, we will need to create a `main.tf` file within our terraform folder. 
```
sudo nano main.tf
```
---
### Please note!!
- You need to have a **separate file for terraform, than the one used when installing the software**.

![](pictures/terra-install.PNG)
- This is the location of the installation folder.

![](pictures/terra-pwd.PNG)
- This is our terraform working folder, where we will create our `mail.tf`.
---
2. Within the `main.tf` file, we will need to introduce the script for the creation of our `EC2` instance.

```
# launch ec2
# which cloud provider - aws
# terraform downloads required dependencies
# terraform init

provider "aws" {
    region = "eu-west-1"
  

}


resource "aws_instance" "app_instance" {
  ami           = "ami-0d5820667b1474236"
  instance_type = "t2.micro"
  associate_public_ip_address = true
  tags = {
      Name = "tech201-florina-terraform-app"
  }
}


```

- In the actual file, it will look something like this:

![](pictures/main-folder.PNG)

- For a better visualization of the code, please use an IDE (I used Visual Studio Code with Terraform installed as an extension).

3. Now that our `main.tf` is set, we are ready to run the commands to launch the `EC2` instance.
- In the `git bash` terminal that has as `pwd` the path to the folder with `main.tf`, run the following:
```
terraform init 
# initialises the software
```
![](pictures/terra-init.jpg)

```
terraform plan 
# check for any errors in the code in the main.tf file
```
![](pictures/terra-init.jpg)

```

terraform apply
# will run the file and the script inside - creating the instance


```

![](pictures/terra-apply.jpg)

- If everything went well, you should be able to see your instance being create on `AWS`.

![](pictures/instance.PNG)

- For cost effectiveness purposes, when you are ready to terminate the instance, please run:
```
terraform destroy
```
![](pictures/terra-destroy.jpg)

- Happy days! We have showcase how easy it is to create `EC2` instances using `Terraform` with just a few lines of code.






---
## Possible blockers 

- You might receive errors specifying `"Access denied!"`. 
- In the case of such error, double check that your `AWS_ACCESS_KEY_ID` and `AWS_SECRET_KEY` are correct! Check them letter by letter if needed!
- Make sure that after any changes made to the keys, you always open a fresh `Git bash` terminal with admin access.
- Also, in some cases, it might help to restart your local machine. This will save the changes made on the `env variables`. 


---

---

## Launchig a VPC and EC2 instances within it with Terraform

- With `Terraform` we can launch all sorts of services with AWS, from `EC2` instances to `VPCs`.
- For that reason, we will create a VPC and instances within this newly launched VPC, by also creating the required dependencies, such as `subnets`, `internet gateway`, `security groups` and `route tables`.
- To achieve all this and more, the best practice is to use the `Terraform documentation` as it is extremely user friendly and can help us create any sort of service, by only copy & pasting the script structure and replacing the values with what is applicable in our case.
- One thing to remember is, we can use `variables` for automation, instead of manually typing different values that might be used in multiple scenarios. 

---
### Example of variables:
- Remember the previously created script for our EC2 instance using an `AMI`?

![](pictures/main-folder.PNG)

- Instead of specifying the `AMI` name every single time we want to use it, we can create a variable that can hold that specific ID for the `AMI`. 
- In order to achieve that, we must create a new file within the same folder, named `variable.tf`.

![](pictures/variable-file.PNG)

- Now that we have created a variable that holds the specific id of the `AMI` we want to use when creating specific `EC2` instances, we can simply call that variable next time we want to use it in the configuration of an `EC2` instance.
- To call a variable as a value, simply use `var.[name-of-the-variable]`.
- To showcase, if we want to call the newly made variable in the previously used script bloce for an `EC2` instance, it will look like this:

![](pictures/variable-in-ec2.PNG)

---

- For the creation of the entire `VPC` with all the respective dependencies, I will showcase the `main.tf` file block-by-block.

1. `VPC` creation block:
 
 ![](pictures/terra-VPC.PNG)

 - As seen in the picture, I needed to specify the the service I am using (`"aws_vpc"`) and the name  I am giving it. 
 - I also had to specify the `CIDR block` (`"10.0.0.0/16"`), and the `Tenancy type` (default).
 - When ready, run:
 ```
terraform plan

terraform apply
 ```

2. `Internet Gateway` creation block:

![](pictures/terra-IG.PNG)

- As I needed this `Internet Gateway` to be applied within my newly created `VPC`, I needed to specify the id of the VPC, which I took from the AWS VPC Dashboard, and created a variable with it (`"vpc_id"`), as you can see in the block of code when I am specifying the value of the vpc_id.

3. `Public Subnet` creation block:

![](pictures/terra-subnet.PNG)


4. `Route Table` for the `Public Subnet` creation block:

![](pictures/terra-RT.PNG)

5. Association of `Public Subnet` to the `Route table` creation block:

![](pictures/terra-association-rt-subnet.PNG)

6. `Security group` for `Public subnet` creation block:

![](pictures/terra-SG.PNG)

7. `EC2` instance within the `VPC` creation block:

![](pictures/terra-ec2.PNG)

