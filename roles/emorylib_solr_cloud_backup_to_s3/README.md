emorylib_solr_cloud_backup_to_s3
=========

This role will backup a list of solr cloud collections to a S3 bucket. The role uses the Solr Cloud backup API which can be found [here](https://lucene.apache.org/solr/guide/6_6/making-and-restoring-backups.html).

Requirements
------------

The solr cloud backup api requires that each solr instance in the cluster be connected to the same share drive. Considering this role is tuned for AWS, my recommendation is running another role I've created: wm_role_mount_efs_volumes (role is not on public github yet).

Role Variables
--------------
```yaml
sc_backup_server: #Server address, defaults to localhost
sc_backup_port: #Server Port
sc_backup_location: #Share drive path
sc_backup_location_cleanup: #Controls where the snapshot. directories on the share drive are deleted, defaults to true

sc_backup_list: #List of solr cloud collections to be backed up
  - name: #Required, name of collection to be backed up
    location: # Optional, path of shared drive, defaults to sc_backup_location
```

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
