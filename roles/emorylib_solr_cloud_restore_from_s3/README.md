emorylib_solr_cloud_restore_from_s3
=========

This role will restore a solr cloud collection, the collection is stored in an s3 bucket and copied down to the server.

Requirements
------------

Solr Cloud Collection restore requires a shared drive between all solr cloud servers.

Role Variables
--------------

```yaml
sc_restore_list:
  - restore_name: #Name of collection to be restored from
    restore_epoch: #Epoch time of restore, can also accept "latest"
    bucket_name: # defaults to sc_restore_bucket_name
    bucket_prefix: #Optional Bucket prefix, defaults to sc_restore_bucket_prefix
    collection_name: #Name of collection to be restored. Optionally will delete a collection that matches this name, then restore

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

Tag
----------------

delete-solr-collection - Will delete the collection_name in sc_restore_list without attempting to restore a collection afterwards

Example Playbook
----------------

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
