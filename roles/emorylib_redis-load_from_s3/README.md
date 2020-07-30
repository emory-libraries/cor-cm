emorylib_redis-load_from_s3

=========

This role uses the redis-dump gem's redis-load executable. It will download a redis dump json file from S3 and load it with user specified options.

Requirements
------------

awscli must be installed on the host executing this role.

Role Variables
--------------

```yaml
### Main Input Variable
redis_load:
  options:                                                           # List of options to be ran with the executable
    - d: 9
    - u: redis-hostname
  s3:                                                                # Required S3 section, includes bucket and possible key prefix
    bucket: bucket-name
    key_prefix: /
    date: 'latest'                                                   # Takes a date or 'latest', dates must be included in quotes
  replace_db: 9                                                      # Optional, if present the role will replace the db field with this
  delete_file: yes                                                   # Delete file after restoring, defaults to true
```

Example Playbook

----------------
```yaml
### Restore single redis database
- hosts: all
  vars:
    redis_load:
      options:
        - d: 1
        - u: redis-host-003.com
      replace_db: 1
      s3:
        bucket: backup-bucket
        key-prefix: redis/db/2
        date: '2020-07-26T23:49:03.355197Z'
### Restore all redis databases
  vars:
    redis_load:
      options:
        - u: redis-host-003.com
      s3:
        bucket: full-redis-backup
        key_prefix: /
        date: 'latest'
  tasks:
    - include_role:
        name: emorylib_redis-load_from_s3
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
