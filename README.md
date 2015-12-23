# CouchDB Ansible Role

Installs CouchDB on CentOS 7 or Fedora 22.

## Requirements

The following roles are required:

* [facts](https://github.com/idi-ops/ansible-facts)

## Role Variables

    couchdb_create_admin_user: false

Whether an administrator account should be created. In order to enable this the two `couchdb_admin_username` and `couchdb_admin_password` variables need to be declared.

    couchdb_admin_username:
    couchdb_admin_password:

CouchDB administrator user name and password.

    couchdb_username: 'couchdb'
    couchdb_groupname: 'couchdb'

CouchDB system accounts that are used by the couchdb daemon and own all required directories.

    couchdb_port: '5984'

Default port used by the daemon.

    couchdb_host_address: '0.0.0.0'

The IP address that the service should listen on.

    couchdb_databases: []

List of databases to create.

    couchdb_delete_existing_databases: false

Whether databases defined using ``couchdb_databases`` should be deleted if they exist. This is most likey only useful in development environments where you may want to remove existing data and start from scratch.

    couchdb_enable_automatic_compaction: true

Whether the database(s) should be compacted.

    couchdb_check_interval: '300'

The delay, in seconds, between each check for which database and view indexes need to be compacted.

    couchdb_min_file_size: '131072'

If a database or view index file is smaller then this value (in bytes), compaction will not happen. Very small files always have a very high fragmentation therefore it's not worth to compact them.

    couchdb_compaction_start_time: '23:00'
    couchdb_compaction_end_time: '04:00'

The period within which database compaction is allowed. The value for these parameters must obey the format:

HH:MM - HH:MM (HH in [0..23], MM in [0..59])

couchdb_test_http_status_code: '200'

The status code expected from making a request to CouchDB.

couchdb_test_string: 'Welcome'

A string to search for in the response body returned by the test request. This, along with ``couchdb_test_http_status_code``, are only used by the ``test`` target and playbook.

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
