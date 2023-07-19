# Ansible: File Module

## Create folder
```yaml
- name: ansible set permission recursvely for a directory
  file:
    path: {{ item }}
    state: [file,directory,hard,link]
    mode: "u=rw,g=wx,o=rwx"
    recurse: yes
  with_items:
    - '/tmp/devops_system1'
    - '/tmp/devops_system2'
    - '/tmp/devops_system3'
```

## Create Folder Dynamically
```yaml
- name: ansible create directory with_items example
  file:
    path: "{{ item.dest }}"
    mode: "{{item.mode}}"
    state: [file,directory,hard,link]
  with_items:
    - { dest: '/tmp/devops_system1', mode: '0777'}
    - { dest: '/tmp/devops_system2', mode: '0707'}
    - { dest: '/tmp/devops_system3', mode: '0575'}
```

## Deleting Folder
```yaml
- name: ansible remove directory example
    file:
      path: /tmp/devops_directory
      state: absent
```

## Setting owner and group
```yaml
- name: Setting file group and owner
  file: 
    dest:/foo/bar/somedir 
    owner:root 
    group:nginx 
    mode: "u=rwX,g=rX,o=rX" 
    recurse=yes
```

