# Vagrant-Ansible

![](pictures/ansible-terraform-vagrant.PNG)

- First of all, we all have Virtual Box on our local hosts, and we know how to use Vagrant.
- Compared to previous times, in order to be able to use Ansible, we will need to create 3 VMs. This is the minimum requirement for Ansible.
- One of them will be an Ansible Controller, and the other 2 will be web (for nodejs) and db (for mongodb).
- Previously, we needed to manually SSH in each machine (web and db).
- We also had to SSH into the machines to install the other dependencies.
- With Jenkins, we managed to automate the entire process of using our code to test it and automate the process of creating instances with installed dependencies without having to SSH into them. 
- However, the Cloud end of the process is not automated, as we do not know how to automate the creation of EC2 instances that will be provisioned by Jenkins.
- This is where Ansible comes in place. 
- As setting up the Cloud bit consumes money, we will focus on setting up everything locally first, and then proceed with the Cloud end.
- Locally, we will create an Ansible Controller with 2 agent nodes (web and db VMs).
- As Ansible is `Agentless`, we will be able, from the Ansible Controller to ssh into the 2 agent nodes without needing to install Ansible in them as well. We just need to establish the connection between the Ansible Controller and the 2 Agent nodes VMs.
- In order to create the Ansible Controller, we will still need to install dependiencies that will make that particular VM an Ansible Controller, but those dependencies and installations don`t need to be done in the agent Nodes.
- With the help of Ansible, if we run a command in Ansible, we can run the same commands in all the other machines as long as they are set as Agent Nodes.

## Steps:

### Step 1: Creating the VMs
- Create a local folder called `Ansible`.
- Open the folder in `Visual Studio Code` and create a `Vagrantfile`. For the creation of Vagrantfiles, you do not need to add any `.type` at the end of the file. This file becomes a `Vagrantfile` when the script is added to it. 
- Copy the following configuration within your `Vagrantfile`:
```
# ansible-tech201


# -*- mode: ruby -*-
 # vi: set ft=ruby :
 
 # All Vagrant configuration is done below. The "2" in Vagrant.configure
 # configures the configuration version (we support older styles for
 # backwards compatibility). Please don't change it unless you know what
 
 # MULTI SERVER/VMs environment 
 #
 Vagrant.configure("2") do |config|
 # creating are Ansible controller
   config.vm.define "controller" do |controller|
     
    controller.vm.box = "bento/ubuntu-18.04"
    
    controller.vm.hostname = 'controller'
    
    controller.vm.network :private_network, ip: "192.168.33.12"
    
    # config.hostsupdater.aliases = ["development.controller"] 
    
   end 
 # creating first VM called web  
   config.vm.define "web" do |web|
     
     web.vm.box = "bento/ubuntu-18.04"
    # downloading ubuntu 18.04 image
 
     web.vm.hostname = 'web'
     # assigning host name to the VM
     
     web.vm.network :private_network, ip: "192.168.33.10"
     #   assigning private IP
     
     #config.hostsupdater.aliases = ["development.web"]
     # creating a link called development.web so we can access web page with this link instread of an IP   
         
   end
   
 # creating second VM called db
   config.vm.define "db" do |db|
     
     db.vm.box = "bento/ubuntu-18.04"
     
     db.vm.hostname = 'db'
     
     db.vm.network :private_network, ip: "192.168.33.11"
     
     #config.hostsupdater.aliases = ["development.db"]     
   end
 
 
 end
```
- Now that we have our `Vagrantfile`, we need to open `Virtual Box`.
- Open a `Bash` terminal in VSC or an independent `Git Bash` terminal (and navigate to the folder with the `Vagrantfile`) and run:
```
vagrant up
```
---

### Please note:
- If you encounter any issue when attempting to run the Vagrant file, it means you might have a typo in the name of the Vagrant file. **Please, make sure the Vagrant file is named `Vagrantfile` and nothing else.**

---

- The creation of the VMs will take a while as 3 VMs are being created. So, please be patient.
- Once the creation of the VMs is finished, we need to `SSH` into each machine and establish the connection to the internet viat our local host.
- Simply do that by running the following:

1. For the Controller VM
```
vagrant ssh controller

sudo apt-get update -y 

sudo spt-get upgrade -y

exit
```

2. For the Web VM
```
vagrant ssh web

sudo apt-get update -y

sudo apt0get upgrade -y

exit
```

3. For the DB VM
```
vagrant ssh db

sudo apt-get update -y

sudo apt-get upgrade -y

exit
```
- Again, the updates and upgrades might take a long time, depending on your local machine`s internet connection and your internet overall speed. Please, be patient.
- Once all the updates and upgrades are done, please SSH back into the Controller VM.
```
vagrant ssh controller
```
- Once inside the Controller VM, we need to install some dependencies for our system.
```
sudo apt-get install software-properties-common

sudo apt-add-repository ppa:ansible/ansible
# When running this, you might be prompted to ENTER mid-process. If so, pelase press ENTER

sudo apt-get update -y 

sudo apt-get install ansible -y

ansible --version
# to double-check the version of ansible
# although we did not install python, as it is one of the requirements for ansible, it installed it and it is using the default version (python 2.7)

cd etc/

cd ansible/

# this confirms that we have an Ansible Controller set up
```
- Happy days! We set up our `Ansible Controller` and we are ready to move forward into communicating with the other 2 VMs, which will be the Agen Nodes.