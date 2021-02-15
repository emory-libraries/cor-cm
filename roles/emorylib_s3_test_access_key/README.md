emorylib_s3_test_access_key
=========

Small role that will use the supplied AWS Access/Secret key to list a target bucket, thus refreshing the activity on the Access Key, avoiding deletion from automated remediators.
If the supplied key fails, will optionally send an error message to a Slack channel.

Requirements
------------

```yaml
collections:
  - amazon.aws
```

Role Variables
--------------

```yaml
s3_user_name: # Required, user name to associate with key, is reported to slack on failure
s3_user_bucket: # Required, bucket to run list operation against, access key must have Get/List perms on bucket
s3_user_prefix: # Optional but highly recommended if the bucket has a large number of files on the root, create an empty folder and use it as prefix, otherwise this role will take a long time to execute.
s3_user_access_key: # Optional, can also be supplied by Ansible Tower Credential
s3_user_secret_key: # Optional, can also be supplied by Ansible Tower Credential
s3_user_local_support: # Optional, contact that is listed in Slack error message

### Optional Slack variables, mirrors ansible Slack Module
### Minimum required variable for slack to work: s3_user_slack_token

s3_user_slack_token:
s3_user_slack_msg:
s3_user_slack_channel:
s3_user_slack_color:
s3_user_slack_thread_id:
s3_user_slack_username:
s3_user_slack_icon_url:
s3_user_slack_icon_emoji:
s3_user_slack_link_names:
s3_user_slack_parse:

```

Example Playbook
----------------

```yaml
---
- hosts: localhost
  connection: local
  vars:
    
  tasks:
    include_role:
      name: emorylib_s3_test_access_key
```

License
-------

BSD

Author Information
------------------

Created by Solomon Hilliard for Emory Libraries
