emorylib_solr_cloud_restore_from_s3
=========

This role restores a solr cloud collection and optionally sets an alias. The collection sources can be either an s3 bucket or a path on a share drive. The twin backup role for this restore role is emorylib_solr_cloud_backup_to_s3.

Requirements
------------

Restoring a solr cloud collection requires a shared drive between all solr cloud clusters. The role will fail if a shared drive is not detected.

Role Variables
--------------

```yaml
solr_cloud_restore: # Main input variable
  collection:
    name: # Name of the collection to restore
    delete_if_collection_exists: # When attempting to restore a collection that already exists, set this option to yes to delete the existing collection before restoring. Otherwise the role will fail.
    delete_all_matching: # This will search for collections matching a pattern and delete them, the search is done BEFORE the collection is restored, so whatever is being restored will not be deleted even if it matches the pattern.
                         # This is intended to be used for collections with time stamps
  aliases: # List of aliases that will be created and pointed to the restored collection. If the alias already exists, it will be overwritten.
  source: # Defines where the role will search for the backup files, the mutually exclusive options are file and s3
    file:
      path: # Path to a directory which holds backup folders. The folders names should be timestamps
      date: ''  # Date of collection to be restored, needs to match a date found in the path. 'latest' will pick the latest date the script can find. If giving a date, it must be in quotes
      clean_path: # Deletes all subfolders in the path after ingesting the restore, default is no
    s3: # Mutually exclusive with file, this option will query a S3 bucket key,
      bucket: # S3 Bucket
      key_prefix: # Where the backup folders are stored
      tmp_path: # Temporary path where the backup folder will be downloaded to, this must be a share drive reachable by all hosts in the solr cluster
      date: '' # Date of collection to be restored, needs to match a date found in the path. 'latest' will pick the latest date the script can find. If giving a date, it must be in quotes
    

sc_restore_delete_collection: yes # Will delete a collection before attempting to restore it, otherwise the play will fail
sc_restore_bucket_name: # Default bucket name, can be overidden inside of sc_restore list
sc_restore_bucket_prefix: # Default bucket prefix, can be overidden inside of restore list

sc_restore_server: localhost
sc_restore_port: 8983

sc_restore_location: /mnt/efs
sc_restore_location_cleanup: yes # Will clean out the restore directory

sc_restore_temp_path: /tmp
sc_restore_base_url: 'http://{{ sc_restore_server }}:{{ sc_restore_port }}/solr'

#These variables affect how long the play will wait for a restore to be completed
sc_restore_retries: 100
sc_restore_delay: 3
```

Example Playbook
----------------

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
