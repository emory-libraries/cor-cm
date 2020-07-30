emorylib_redis_dump_to_s3
=========

This role will install a ruby gem called redis-dump, then use the gem to backup a redis database. Optionally it will pushed to S3 and/or deleted.

Requirements
------------

Ruby must be installed on the host running this role.

Role Variables
--------------

```yaml
### Main input variable
redis_dump:
  options:                                                          # Optional list of options. Please check the redis-dump documentation for examples on what each option does.
    - d: 1                                                          # d flag specifies that database 1 will be backed up
    - u: redis-hostname-here.com                                    # u specifies the host name of the redis instance
  dest: '/tmp/{{ ansible_date_time.iso8601_micro }}'                # Optional, Where the outputing json file will be created.
  delete_local: yes                                                 # Optional, will delete the local file after it's been uploaded to S3
  s3:
    bucket: s3_bucket_name                                          # Required if using S3
    key_prefix: any_key_prefix_here                                 # Optional Key Prefix
    region: us-east-1                                               # Required region of bucket

### Additional variables
redis_dump_executable_path: ~/bin/redis-dump                        # Controls the path to the redis-dump executable
### Async Variables                                                 # Manages the async call of the role
redis_dump_async: 3000
redis_dump_retries: 300
redis_dump_delay: 10

```

Dependencies
------------

Example Playbook
----------------

```yaml
# Simple run
- hosts: all
  vars:
    redis_dump:
      options:
        - d: 2
        - u: redis_instance.com
      s3:
        bucket: test-bucket
        region: us-west-1
  tasks:
    - include_role:
        name: emorylib_redis_dump_to_s3
```

License
-------

BSD

Author Information
------------------

Solomon Hilliard for Emory Libraries