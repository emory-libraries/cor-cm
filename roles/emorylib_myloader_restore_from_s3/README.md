Role Name
=========

This role download a mydumper backup from s3, then perform a myloader restore with desired options.

Requirements
------------

The remote executing this role must have awscli and boto3 installed.

Role Variables
--------------

```yaml
myloader_restore:
  notify_slack:
  option:
    _B: whatever
  # Can't specify dir flags when using date, otherwise it's same as mydumper
  s3:
    bucket:
  key_prefix:
  date:
  download:             # Optional Key, default is /tmp path and to delete the s3 directory after the myloader restore is complete
    delete: yes
    path: /tmp

myloader_restore_owner: # Optional owner for directory downloaded from S3
myloader_restore_group: # Optional group for directory downloaded from S3

myloader_restore_slack:
myloader_stop_programs:
  - fedora
  - httpd
```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
