## Activate Virtual Env
```
source ~/venvs/ansible/bin/activate
```

## Install Ansible Dependencies (if not already done)
```
pip install pypsrp pywinrm
ansible-galaxy collection install ansible.windows # trippsc2.cis 
```

## Install Azure Dependencies
```
ansible-galaxy collection install azure.azcollection
pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements.txt
```

## Define Env Vars
```
. .env
```

## Ansible Inventory
```
ansible-inventory -i inventory.yml --limit dbservers --graph
ansible-inventory -i inventory.yml --list --yaml
ansible-inventory -i inventory.azure_rm.yml --list --yaml
```

## Ping Windows Server
```
ansible -i inventory.azure_rm.yml -m ansible.windows.win_ping azwu2nhsw001
```

## Ping Linux Server
```
ansible -i inventory.azure_rm.yml -m ansible.builtin.ping ansible01
ansible -i inventory.azure_rm.yml -m ansible.builtin.ping -e "ansible_connection=local" ansible01
```

## Install IIS
```
ansible-playbook -i inventory.azure_rm.yml --limit=windows playbook-install-iis.yml # run twice to test idempotency
```

## Apply CIS Hardening (Using Ansible Role)
```
ansible-playbook -i inventory.azure_rm.yml --limit=windows playbook-windows-cis-hardening.yml
```

## Apply StorageDsc Configuration
```
## install prerequisites
# ansible-galaxy role install -r roles/requirements.yml --force
# ansible-galaxy collection install community.windows
ansible-playbook -i inventory.azure_rm.yml --limit=windows playbook-provision-storage.yml
```

## Provision Ansible Users
```
ansible-playbook -i inventory.azure_rm.yml --limit=ansible01 -e "ansible_connection=local" -e @extra_vars/users.yml playbook-provision-ansible-ssh-users.yml --become
```