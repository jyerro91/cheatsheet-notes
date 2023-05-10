# Ansible Commands

## Basic ansible module
```bash
ansible -m ping all
```

## Get server facts
```bash
ansible -m setup HOST/SERVERNAME/IP
```

## Create roles folder via ansible galaxy
```bash
ansible-galaxy webservers init
```

## Debug Module
```yaml
- name: this is the debug module
  debug:
    msg: "Message"
```
```yaml
- name: Print the package facts
 debug:
   var: ansible_facts.packages
```

## Check mode
###### Test configuration without deploying any changes
```bash
ansible-playbook playbook --check
```

## Create ansible vault data file
```bash
ansible-vault create file.yml
```
