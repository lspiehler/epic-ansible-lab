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
ansible -i inventory.yml -m ansible.windows.win_ping azwu2nhsw001
```

## Install IIS
```
ansible-playbook -i inventory.yml playbook-install-iis.yml # run twice to test idempotency
```

## Apply CIS Hardening (Using Ansible Role)
```
ansible-playbook -i inventory.yml playbook-windows-cis-hardening.yml
```
