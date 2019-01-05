## Instructions to start "Vagrantfile_pairing" vagrant box
Vagrantfile_pairing creates a CentOS/7 environment that hosts a number of development tools. These tools meet the requirements [here](https://github.com/EqualExperts/interview-prep/tree/master/devops). Ensure that you meet the "Vagrant Box prerequisites" listed in [README.md](./README.md) before attempting to start the vagrant box.

Spin up the VM by executing the command below :
- `export VAGRANT_VAGRANTFILE=Vagrantfile_pairing`
- `clear && vagrant destroy --force && vagrant up --color`

All resources developed during the pair programming exercise will be stored in the ./Vagrantfile_pairing_folder

### Docker container usage
If you need a tool that is not available in this environment, you may use that tool in a container form. As an example, see the command below for [Maven](https://hub.docker.com/_/maven):
- `alias mvn='docker run -it --rm -v $HOME/.m2:/root/.m2 -v $PWD:/usr/src/maven -w /usr/src/maven maven mvn'`
- `mvn --version`
- `sudo chown -R $USER:$USER $HOME/.m2`

### More examples of using containers afer cloning the code:
- `git clone https://github.com/shazChaudhry/Equal_Experts_tech_test.git`
- `cd Equal_Experts_tech_test`

An other example Test Ansible role in a container
- `docker container stop $(docker container ps -aq)` _(This will stop all running containers)_
- `docker container rm $(docker container ps -aq)` _(This will remove all containers)_
- `docker container run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v $PWD:/tmp -w /tmp/ansible/roles/install-jenkins quay.io/ansible/molecule:latest sudo molecule test`
