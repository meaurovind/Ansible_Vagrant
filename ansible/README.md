## prerequisites
- CentOS/7
- Latest version of [Docker CE for CentOS](https://docs.docker.com/install/linux/docker-ce/centos/) _(container will be used to test and run Ansible roles)_

## Clone this repo
This alias is only required if git is not already installed on your machine. This alias will allow you to clone the repo using a git docker container
- `alias git='docker run -it --rm --name git -v $PWD:/git -w /git alpine/git'`
- `git version`
- `git clone https://github.com/shazChaudhry/Equal_Experts_tech_test.git`
- `sudo chown -R $USER:$USER Equal_Experts_tech_test`

## Jenkins server setup
- `cd Equal_Experts_tech_test`
- 

## Running individual Molecule commands
You will need to update relevant files under molecule directory and then execute the role inside a docker container as follows _(install-jenkins role is assumed here)_. You will also need to ensure that all existing container have stoped and removed: `docker container stop $(docker container ps -aq)`.
- lint - Executes yaml-lint, ansible-lint, and flake8, reporting failure if there are issues
- syntax - Verifies the role for syntax errors
- create - Creates an instance with the configured driver
- prepare - Configures instances with preparation playbooks
- converge - Executes playbooks targeting hosts
- idempotence - Executes a playbook twice and fails in case of changes in the second run (non-idempotent)
- verify - Execute server state verification tools (testinfra or goss)
- destroy - Destroys instances
- test - Executes all the previous steps
> The `login` command can be used to connect to provisioned servers for troubleshooting purposes

## Running all tests using Docker
You will need to update relevant files under molecule directory and then run the role inside a docker container as follows _(deploy-nginx role is assumed here)_. You will also need to ensure that all existing container have stoped and removed: `docker container stop $(docker container ps -aq)`
```
          docker run --rm -it \
          -v $PWD:/tmp/$(basename "${PWD}"):ro \
          -v /var/run/docker.sock:/var/run/docker.sock \
          -w /tmp/$(basename "${PWD}") \
          quay.io/ansible/molecule:latest \
          sudo molecule test
```

## References
- quay.io/repository/ansible/molecule - Docker container used for testing Ansible roles
- molecule.readthedocs.io - Molecule is designed to aid in the development and testing of Ansible roles. Molecule provides support for testing with multiple instances, operating systems and distributions, virtualization providers, test frameworks and testing scenarios.
- github.com/aelsabbahy/goss - Goss is a YAML based serverspec alternative tool for validating a server’s configuration. It eases the process of writing tests by allowing the user to generate tests from the current system state.
