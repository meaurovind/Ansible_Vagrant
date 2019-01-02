## Prerequisites
It is assumed you are using CentOS/7 and that the following tools are installed:
- Latest version of [Docker CE for CentOS](https://docs.docker.com/install/linux/docker-ce/centos/). This is optional if you do not intend to test the Jenkins role
- Latest version of [Molecule](https://molecule.readthedocs.io/en/latest/installation.html#centos-7). This is optional if you do not intend to test the Jenkins role
- Be sure to install python docker module _([see github issue](https://github.com/ansible/molecule/issues/1383))_:
  - `sudo pip install docker`
- Latest version of Git
- Latest version of Ansible

## Clone this repo
- `git clone https://github.com/shazChaudhry/Equal_Experts_tech_test.git`
- `cd Equal_Experts_tech_test/ansible`

## Install dependant roles
Newer versions of Jenkins require Java 8+ and so it must be installed before executing "install-jenkins" role. Download OpenJDK 8 by executing the command below:
- `ansible-galaxy install -r roles/requirements.yml`

## Install Jenkins
Running the main playbook will ensure that all dependant galaxy roles are downloaded and installed before Jenkins is installed
- `ansible-playbook site.yml`

## Jenkins URL
The web URL serving out the Jenkins login page should be accessible at [http://techtest:8080/jenkins](http://techtest:8080/jenkins)
  - username = `admin`
  - password = `admin`

## Running individual Molecule commands
Docker and Molecule are prerequisites to test roles. Please ensure these tools are already installed.

You will need to update relevant files under molecule directory and then may execute individual stages of the molecule workflow below. Change directory to the role to be tested and then run `molecule [COMMAND]` where list of COMMANDs are shown below:

- Ensure all existing container have stoped and removed:
  - `docker container stop $(docker container ps -aq)`
  - `docker container rm $(docker container ps -aq)`
- Also, ensure Jenkins is not running:
  - `sudo systemctl stop jenkins`
  - `sudo systemctl disable jenkins`
- Change the directory to the role to be tested:
  - `cd roles/install-jenkins`
- lint - Executes yaml-lint, ansible-lint, and flake8, reporting failure if there are issues
- syntax - Verifies the role for syntax errors
<!-- - create - Creates an instance with the configured driver
- prepare - Configures instances with preparation playbooks -->
- converge - Executes playbooks targeting hosts
<!-- - idempotence - Executes a playbook twice and fails in case of changes in the second run (non-idempotent) -->
- verify - Execute server state verification tools (goss)
- destroy - Destroys instances
<!-- - test - Executes all the previous steps -->
> The `login` command can be used to connect to provisioned servers for troubleshooting purposes

## References
- [quay.io/repository/ansible/molecule](https://quay.io/repository/ansible/molecule) - Docker container used for testing Ansible roles
- [molecule.readthedocs.io](https://molecule.readthedocs.io/en/latest/) - Molecule is designed to aid in the development and testing of Ansible roles. Molecule provides support for testing with multiple instances, operating systems and distributions, virtualization providers, test frameworks and testing scenarios.
- [github.com/aelsabbahy/goss](https://github.com/aelsabbahy/goss) - Goss is a YAML based serverspec alternative tool for validating a server’s configuration. It eases the process of writing tests by allowing the user to generate tests from the current system state.
