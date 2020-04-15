emorylib_solr_cloud_backup_to_s3
=========

This role will backup a solr cloud collection. The backup will output to a file path and/or to a S3 bucket. The role uses the Solr Cloud backup API which can be found [here](https://lucene.apache.org/solr/guide/6_6/making-and-restoring-backups.html).

The role is tuned to have the collection name be the key or folder, the directory of the backup itself, a time stamp. Default time stamp is iso8601 micro.

Requirements
------------

The solr cloud backup api requires that each solr instance in the cluster be connected to the same share drive. Considering this role is tuned for AWS, my recommendation is running another role I've created: wm_role_mount_efs_volumes (role is not on public github yet).

Role Variables
-------------

```yaml
solr_cloud_backup:  # Main input variable
  collection:
    name: # Name of the collection to be backed up, this will form the outer directory of file backups
  dest: # either .path or .s3 must be specified
    path: # Backup will be copied to this location, the collection name will be the outer directory. Must be different from tmp_path
    s3:
      bucket: # Bucket the backup will be synched to
      key_prefix: # Key prefix that the timestamped backup directory will be made in, strongly suggest making the last key the collection name
      region: # Region of the bucket
      aws_access_key: # Optional
      aws_secret_key: # Optional
      security_token: # Optional
  tmp_path: # Path given to Solr to create the backup in, this must be a shared drive accessable to all solr cloud instances

solr_cloud_backup_owner: # user who owns the solr backup directory, default is solr
solr_cloud_backup_group: # group who owns the solr backup directory, default is solr

solr_cloud_backup_post_params: #Default parameters for the solr cloud backup job, modify at your own risk
solr_cloud_backup_server: # forms the base url, default is localhost
solr_cloud_backup_port: # forms the base url, default is 8983

solr_cloud_backup_base_url: # url path to admin, default is 'http://{{ solr_cloud_backup_server }}:{{ solr_cloud_backup_port }}/solr/admin'
solr_cloud_backup_retries: # number of retries to wait for the solr cloud job to finish writing, you may have to increase for larger solr cloud backups, default is 100
solr_cloud_backup_delay: # number of seconds between each retry, you may have to increase for larger solr cloud backups, default is 10
```

Example Playbook
----------------

```yaml
    - hosts: solr-instance-1 # Only run against a single solr cloud instance
      vars:
        # This example will backup to a directory and s3
        solr_cloud_backup:
          collection:
            name: solr_collection
          dest:
            path: /mnt/archive/drive
          tmp_path: /mnt/solr_shared_drive/backup
          s3:
            bucket: solr-backup-bucket
            region: us-east-1
            key_prefix: solr-group-name/solr_collection # Recommended S3 bucket path
      roles:
         -  emorylib_solr_cloud_backup_to_s3
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
