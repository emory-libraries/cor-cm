Role Name
=========

This role will download a folder or file from s3 to a local filesystem. Please carefully read the variable section for instructions on how to use this role.

Requirements
------------

The remote running this role must have awscli and boto3 installed via pip.

Role Variables
--------------

```yaml
# Main Input Variable
copy_from_s3:
  local:
    path: /file-system/path/here                                           # Required, path on local filesystem where the s3 key will be downloaded to
    if_path_exists: stop                                                   # Optional, controls what happens if the local path already exists. Options are stop, delete and rename
  follow_symblink:                                                         # Optional, determines whether the stat check should follow symblinks or not, default is omit
  s3:
    bucket:
    key_prefix:
    date:
  escalate:                                                                # Optional key, omitted by default, the sub-keys follow the same functionality as standard Ansible
    become:
    become_user:
    become_method:

# Extra variables
confirm_delete_retries: 100
confirm_delete_delay: 30
```

Dependencies
------------


Example Playbook
----------------

License
-------

BSD

Author Information
------------------

