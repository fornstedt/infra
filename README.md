# infra
An Ansible playbook to setup my home infrastructure. Very inspired by [notthebee's corresponding repo](https://github.com/notthebee/infra).

It assume fresh installed servers, access to a non-root user with sudo privileges and a public SSH key. Currently suited for any of these distributions on the servers:

* Ubuntu Server 20.04
* Arch Linux 2022.01.01

The playbook is currently a work in progress and does not produce a complete system yet.

## Usage
Install Ansible (macOS):
```
brew install ansible
```
Clone the repository:
```
git clone https://github.com/notthebee/infra
```
Create a Keychain item for your Ansible Vault password (on macOS):
```
security add-generic-password \
               -a YOUR_USERNAME \
               -s ansible-vault-password \
               -w
```

The `pass.sh` script will extract the Ansible Vault password from your Keychain automatically each time Ansible requests it.

Create an encrypted `secret.yml` file and adjust add variables found at the bottom of `groups_vars/all/vars.yml`:
```
ansible-vault create group_vars/secret.yml
ansible-vault edit group_vars/secret.yml
```

Add your custom inventory file to `hosts`:
```
cp hosts_example hosts
vi hosts
```

Finally, run the playbook:
```
ansible-playbook run.yml [-l host-name] -K
```
The `-K` parameter is only necessary for the first run, since the playbook configures passwordless sudo for the main login user.

For consecutive runs, if you only want to update the Docker containers, you can run the playbook like this:
```
ansible-playbook run.yml --tags="docker,containers"
```
