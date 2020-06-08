emorylib_sync_s3_buckets
=========

This role with sync the contents of two s3 buckets and optionally report the results to slack

Requirements
------------

The remote running this role must have awscli and boto3 installed via pip.

Role Variables
--------------

```yaml
# Main Input Variable
sync_s3_bucket:
  name:                                       # Optional, name to associate job with; this will be used when reporting to slack
  source_s3:
    bucket:                                   # Required, the bucket of the s3 to be synced from
    key_prefix:                               # Optional, key prefix if syncing a specific folder is desired
  dest_s3:
    bucket:                                   # Required, the bucket of the s3 to be synced to
    key_prefix:                               # Optional, key prefix if syncing to a specific folder is desired
  notify_slack: no                            # Optional, output results of job to slack, requires the sync_s3_bucket_slack variable to be set

# Slack
sync_s3_bucket_slack:                         # Follows the same logic as the ansible slack module. Only requirement is the token. There is a default sync_s3_bucket_slack_attachments
                                              # variable that can be overriden by sync_s3_bucket_slack.attachments if desired
```

Dependencies
------------


Example Playbook
----------------

```yaml
# Sync buckets and report to slack
sync_s3_bucket:
  name: Sync Bucket A to Bucket B
  source_s3:
    bucket: bucket-A
  dest_s3: bucket-B
  notify_slack: yes
sync_s3_bucket_slack:
  token: 'slack/token/to/desired/channel/here'       # By default number of objects and estimated size of sync are displayed in report
```
License
-------

BSD

Author Information
------------------

Solomon Hilliard for Emory Libaries