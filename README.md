[![Build Status](https://travis-ci.org/shazChaudhry/Equal_Experts_tech_test.svg?branch=master)](https://travis-ci.org/shazChaudhry/Equal_Experts_tech_test)

## Requirements
**Vagrant CI** - Using a configuration management language of your choosing (Ansible, Puppet, Salt, Chef) setup a Continuous Integration server of your choosing on a Vagrant Box. You are not required to setup Vagrant or its requirements on the host machine.

Please provide a solution that you would provide to a paying client. This should be delivered zipped up and ready to run. Do include a comprehensive ReadMe file.

## The solution
The primary objective in this solution is to setup a Jenkins server using Ansible:
- This Continuous Integration server will run on CenOS/7 only in order to keep the solution simple
- The Ansible role developed in this solution will be tested with [Molecule](https://molecule.readthedocs.io/en/latest/) and [Goss](https://github.com/aelsabbahy/goss)

The assumption here is that this solution will be developed on a Windows 10 pro machine in wich case you will need to follow the instructions provided below to provision (and teardown) the necessary infrastructure. If however, you alraedy have your own CentOS/7 based infrastructure then you may skip the Vagrant Box related sections below and jump straight to the **[./ansible/README.md](./ansible/README.md)** page.

### Vagrant Box prerequisites
- The user has admin privileges on the development machine
- At least 3GB of free RAM is available on the machine. Otherwise, Vagrantfile will need editing to adjust the available memory:
  - `v.customize ["modifyvm", :id, "--memory", <MEMORY_ALLOCATION>]`
- Latest version of [Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Latest version of [Git for Windows](https://git-scm.com/downloads)
- Latest version of [Vagrant](https://www.vagrantup.com/intro/getting-started/install.html)
  - Install Vagrant Host Manager plugin by running the following command in the Git Bash terminal. This plugin updates the host files on both guest and host machines:
    - `vagrant plugin install vagrant-hostmanager`
  - Install vagrant-vbguest plugin by running the following command. See the comments in [Vagrantfile](./Vagrantfile) regarding shared folders:
    - `vagrant plugin install vagrant-vbguest`

### Vagrant Box provisioning
  1. Clone this repo and change the directory:
      1. `git clone https://github.com/shazChaudhry/Equal_Experts_tech_test.git`
      1. `cd Equal_Experts_tech_test`
  1. Launch the guest machine:
      1. `clear && vagrant up --color`
  1. Should you have a need to SSH to vagrant box and run ansible playbook _(e.g. troubleshooting)_, then run the following commands:
      1.  `vagrant ssh`
      1. `cd /vagrant/ansible`
      1. `export ANSIBLE_CONFIG=./ansible.cfg`. Without this system variable, you may get an error / warning saying _Ansible is being run in a world writable directory_
      1. - `ansible-playbook site.yml`

### Testing
- Jenkins as a service starts up as part of Vagrant Box provisionning. The web URL serving out the Jenkins login page should be accessible at [http://techtest:8080/jenkins](http://techtest:8080/jenkins)
  - username = `admin`
  - password = `admin`
- For testing the Ansible role , please see [./ansible/README.md](./ansible/README.md) page

### Vagrant Box teardown
Once finished, change the directory in the Git Bash terminal to where this repo was cloned and run the following command:
  - `vagrant destroy --force`

## References
[Testing your Ansible roles with Molecule](https://www.jeffgeerling.com/blog/2018/testing-your-ansible-roles-molecule). I found this blog by Jeff Geerling to be very informative on testing Ansible roles with Molecule
