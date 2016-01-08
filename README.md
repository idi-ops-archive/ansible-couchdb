# CouchDB Ansible Role

Installs CouchDB on CentOS 7 or Fedora 22.

## Requirements

The following roles are required:

* [facts](https://github.com/idi-ops/ansible-facts)

## Role Variables

Please refer to the [defaults/main.yml](https://github.com/idi-ops/ansible-couchdb/blob/master/defaults/main.yml) file for a list of variables along with additional documentation.

## Example

Create a ``vars.yml`` file with the following contents:

```
---
couchdb_create_admin_user: true

couchdb_admin_username: "{{ lookup('env', 'COUCHDB_ADMIN_USERNAME') | default('', true) }}"

couchdb_admin_password: "{{ lookup('env', 'COUCHDB_ADMIN_PASSWORD') | default('', true) }}"

couchdb_databases:
  - user

couchdb_delete_existing_databases: false
```

An example playbook that would use ``vars.yml`` above:

```
---
- hosts: localhost
  user: root

  vars_files:
    - vars.yml

  roles:
    - facts
    - couchdb
```

The playbook can then be used like so:

```
COUCHDB_ADMIN_USERNAME=awesome-name \
COUCHDB_ADMIN_PASSWORD=awesome-password \
sudo -E ansible-playbook couchdb-playbook.yml --tags="install,configure,deploy"

sudo ansible-playbook couchdb-playbook.yml --tags="test"
```
