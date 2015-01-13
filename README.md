# CouchDB Ansible Role

Installs CouchDB on CentOS 7. Tested with Ansible 1.8.2

## Requirements

The EPEL repository needs to be enabled.

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
