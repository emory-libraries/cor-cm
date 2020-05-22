Role Name
=========

This role downloads a mydumper backup from s3, then performs a myloader restore with desired options.

Requirements
------------

The remote executing this role must have awscli and boto3 installed.

Role Variables
--------------

```yaml
myloader_restore:
  notify_slack: no      # Optional sends report to slack, otherwise report is only outputted to console
  option:               # Command line options or flags, underscores are converted into dashes
    _B: database_name   # Example flag, this one will get the database name, __database would also have worked
  s3:
    bucket:
    key_prefix:
    date: ''            # Date can be either latest, script will attempt to get latest backup in folder, or specific date, if data is given it must be in quotes
  download:             # Optional Key, default is /tmp path and to delete the s3 directory after the myloader restore is complete
    delete: yes
    path: /tmp

myloader_restore_owner: # Optional owner for directory downloaded from S3
myloader_restore_group: # Optional group for directory downloaded from S3

myloader_become: yes    # Controls whether to escalate when shutting down programs in the myloader_stop_programs list
myloader_become_user:   # Optional user to escalate as
myloader_stop_programs: # Optional list of programs to shutdown before performing the myloader command, the programs are started after the restore is completed

myloader_restore_slack: # Optional slack module, follows the ansible slack module, myloader_restore_slack_msg is the default message variable that can be overridden by myloader_restore_slack.msg
```

Dependencies
------------

awscli
boto3

Example Playbook
----------------

```yaml
# Example using s3 download
- hosts: hosts_with_db_connection
  vars:
    myloader_restore:
      option:
        _B: database_name
        _u: database_user_name
        _p: database_password
        __host: host_url
        _o: # Overwrite tables, no value is needed for this flag
      s3:
        bucket: s3_bucket
        key_prefix: path/to/directories/containing/backups
        date: 'latest'
    myloader_stop_programs:
      - httpd
      - sidekiq
    myloader_restore_slack:
      token: slack/token/here
      channel: '#channel_name'
# Alterative myloader_restore using local backup, not s3 download
  myloader_restore:
    option:
      __directory: /path/to/local/download/folder
      _B: database_name
      _u: database_user_name
      _p: database_password
      __host: host_url
      _o: # Overwrite tables, no value is needed for this flag
```

License
-------

BSD

Author Information
------------------

Solomon Hilliard for Emory Libraries
