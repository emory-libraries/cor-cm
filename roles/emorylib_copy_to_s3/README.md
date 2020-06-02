Role Name
=========

This is a simple role that will copy a path on a local machine to an S3 bucket. It can be called standalone or from another role.

Requirements
------------

The role uses the aws s3 sync command so the remote must have awscli and boto3 installed.

Role Variables
--------------

There are two variables, a main input variable and an optional slack variable. The slack variable follows the slack module.

```yaml
# Main Input variable
copy_to_s3:
  name:                             # Optional, name to associate with the copy job used in reporting
  path:                             # Required, the absolute path of the file or folder to be copied to S3.
  notify_slack: no                  # Optional, controls whether the job will output it's report to slack, default is false
  s3:                               # Required, While the s3 key is required it does have optional sub-keys.
    bucket:                         # Required, s3 bucket which the files or folder will be copied into
    key_prefix:                     # Required, "folder" path in s3 that the files/folder will be copied into
    timestamp: no                   # Optional, default is no timestamp, if changed to yes the timestamp will be appended to the end of the key prefix
    timestamp_format: iso8601_micro # Optional, default is iso8601_micro
  escalate:                         # Optional key, omitted by default, the sub-keys follow the same functionality as standard Ansible
    become:
    become_user:
    become_method:
## Slack Variable
copy_to_s3_slack:                   # Follows same functionality as the Slack module in standard Ansible.
```

Example Playbook
----------------

```yaml
# Copy a folder to s3, no timestamp or slack
copy_to_s3:
  name: Job Name Here
  path: path/to/folder/
  s3:
    bucket: s3-bucket-name
    key_prefix: /

### More complex example:
copy_to_s3:
  path: path/to/file
  s3:
    bucket: another_bucket_name
    key_prefix: key/prefix/path
    timestamp: yes
  notify_slack: yes
  escalate:
    become: yes
copy_to_s3_slack:
  token: token/goes/here
  channel: '#file-backup-channel-name'
```

License
-------

BSD

Author Information
------------------

Solomon Hilliard for Emory Libraries