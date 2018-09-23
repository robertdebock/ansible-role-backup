backup
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-backup.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-backup)

Provides backup for your system.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

The "other part" of this role is the [restore role](https://galaxy.ansible.com/robertdebock/restore). Here is an example of the two working together.

backup.yml:
```
- name: backup
  hosts: all:!localhost

  roles:
    - role: robertdebock.backup
      backup_objects:
        - name: home
          type: directory
          source: /home
        - name: mydatabase
          type: mysql
          source: mydatabase
```

restore.yml:
```
- name: restore
  hosts: all:!localhost
  
  roles:
    - robertdebock.restore
      restore_objects:
        - name: home
          type: directory
          destination: /
        - name: mydatabase
          type: mysql
          destination: mydatabase
```

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/backup.png "Dependency")

Requirements
------------

- A system installed with required packages to run Ansible. Hint: [bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap).
- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the last 3 release of Ansible.)

Role Variables
--------------

- backup_objects:
    - name: home
      type: directory
      source: /home

Dependencies
------------

- None known.

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|
|------------|-----------|-----------|-----------|
|alpine-edge|yes|yes|yes|
|alpine-latest|yes|yes|yes|
|archlinux|yes|yes|yes|
|centos-6|yes|yes|yes|
|centos-latest|yes|yes|yes|
|debian-latest|yes|yes|yes|
|debian-stable|yes|yes|yes|
|fedora-latest|yes|yes|yes|
|fedora-rawhide|yes|yes|yes|
|opensuse-leap|yes|yes|yes|
|opensuse-tumbleweed|yes|yes|yes|
|ubuntu-artful|yes|yes|yes|
|ubuntu-latest|yes|yes|yes|

Example Playbook
----------------

```
---
- name: backup
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.backup
      backup_objects:
        - name: home
          type: directory
          source: /home
```

Nota bene: This role is not idempotent, because it's a list of actions, not a state.

To install this role:
- Install this role individually using `ansible-galaxy install robertdebock.backup`

Sample roles/requirements.yml: (install with `ansible-galaxy install -r roles/requirements.yml
```
---
- name: robertdebock.bootstrap
- name: robertdebock.backup
   
```

License
-------

Apache License, Version 2.0

Author Information
------------------

[Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
