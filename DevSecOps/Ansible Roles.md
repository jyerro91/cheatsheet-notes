# Ansible Roles


## Create Ansible Structured Roles
> This create roles inside the "roles/" folder
```bash
ansible-galaxy init roles/ROLENAME
```

## Install roles from ansible-galaxy repository
```bash
ansible-galaxy install roles/ROLENAME-FROM-Galaxy
```

## Install roles from requirements lists
> Create requirements.yml
> requirements list acts as composer in php
```yaml
---
roles:
  - name: geerlingguy.repo-epel
    version: 3.0.0
  - name: geerlingguy.repo-remi
    version: 2.0.1
  - name: geerlingguy.security
    version: 2.0.1
  - name: geerlingguy.firewall
    version: 2.5.0
  - name: geerlingguy.ntp
    version: 2.2.0
  - name: geerlingguy.git
    version: 3.0.0
  - name: geerlingguy.nginx
    version: 2.8.0
  - name: geerlingguy.php-versions
    version: 4.0.4
  - name: geerlingguy.php
    version: 4.5.0
  - name: geerlingguy.php-mysql
    version: 2.8.0
```
> 