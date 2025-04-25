## Goal
Provision and maintain a Django application with a PostgreSQL DB using Docker.

## Roles used
* `coopdevs.sys-admins-role` to manage the sys admin users
* `geerlingguy.security` to configure security
* `geerlingguy.docker` to configure Docker and Docker Compose
* `django` to manage Docker and the Django instance

## Requirements
### Pyenv and virtualenv

We use Pyenv and Virtualenv to manage the Python version and isolate the project packages and dependencies:

1. Install Pyenv + Virtualenv
```bash
curl https://pyenv.run | bash
```
2. Install Python
```bash
pyenv install 3.12
```
3. Create a virtualenv:
```bash
pyenv virtualenv 3.12 django-provisioning
```
4. Install Ansible and Galaxy dependencies:
```bash
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

## Usage
The flag -l defines the environment where you will execute the provisioning (dev, test, prod).  
You can find the definitions in the hosts file.


## Development
1. Execute SysAdmin playbook as root user the first time (:warning: Only the first time :warning:):
```bash
# First time
ansible-playbook playbooks/sys_admins.yml -l dev -u root

# Next times
ansible-playbook playbooks/sys_admins.yml -l dev
```
2. Execute Provision
```bash
ansible-playbook playbooks/provision.yml -l dev
```


## Testing
In the testing environment the sys_admins playbook is already run.  
For deployment use the tag "django", the rest of the roles wouldn't be necessary to execute.  
There are encrypted variables, so you should use the decrypt ansible password.

1. Execute Provision
```bash
ansible-playbook playbooks/provision.yml -l test --tags=django --ask-vault-pass
```

