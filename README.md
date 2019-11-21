backup
=========

<img src="https://docs.ansible.com/ansible-tower/3.2.4/html_ja/installandreference/_static/images/logo_invert.png" width="10%" height="10%" alt="Ansible logo" align="right"/>
<a href="https://travis-ci.org/robertdebock/ansible-role-backup"> <img src="https://travis-ci.org/robertdebock/ansible-role-backup.svg?branch=master" alt="Build status"/></a> <img src="https://img.shields.io/ansible/role/d/29899"/> <img src="https://img.shields.io/ansible/quality/29899"/>

The purpose of this role is to make backups of your system.

Example Playbook
----------------

This example is taken from `molecule/resources/playbook.yml`:
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  vars:
    backup_cleanup: no

  roles:
    - robertdebock.backup
```

The machine you are running this on, may need to be prepared.
```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - robertdebock.bootstrap
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for backup

# The directory on the Ansible controller where to store backups.
backup_directory: backups

# The directory on the Ansible managed node where to temporarily store backups.
backup_remote_directory: /tmp

# Cleanup files created on the {{ backup_remote_directory }} when done?
backup_cleanup: yes

# What timestamp format to use when saving files.
backup_timestamp: "{{ ansible_date_time.date }}"

# What compression type to use, choose from gz, zip, bz2
backup_format: gz

# A list of objects to backup. Each item has these parameters:
# - name: A name of the object.
# - type: either "directory" or "mysql".
#   - "directory" will archive all files and directories found in the "source".
#   - "mysql" will make a mysql dump of the "source".
# - source: either
#   - a path (when type == directory) or
#   - a database name (when type == mysql )
# - format: either gz, zip of bz2
#
# For example:
# backup_objects:
#   - name: home
#     type: directory
#     source: /home
#   - name: drupal
#     type: mysql
#     source: drupal
#     format: zip

backup_objects:
  - name: varspool
    type: directory
    source: /var/spool
```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.mysql

```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/backup.png "Dependency")


Compatibility
-------------

This role has been tested on these [container images](https://hub.docker.com/):

|container|tag|allow_failures|
|---------|---|--------------|
|amazonlinux|1|no|
|amazonlinux|latest|no|
|alpine|latest|no|
|alpine|edge|yes|
|debian|unstable|yes|
|debian|latest|no|
|centos|7|no|
|centos|latest|no|
|fedora|latest|no|
|fedora|rawhide|yes|
|opensuse|latest|no|
|ubuntu|latest|no|

This role has been tested on these Ansible versions:

- ansible>=2.8, <2.9
- ansible>=2.9
- git+https://github.com/ansible/ansible.git@devel




Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-backup) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-backup/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

Modules
-------

This role uses the following modules:
```yaml
---
- archive
- fetch
- file
- include_tasks
- mysql_db
```

License
-------

Apache-2.0


Author Information
------------------

[Robert de Bock](https://robertdebock.nl/)
