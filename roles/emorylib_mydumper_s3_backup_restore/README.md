Role Name
=========

This role will perform a mysql dump using the 3rd party tool called mydumper. The backup folder will be pushed to s3 and/or a path on the file system.

Requirements
------------

Mydumper must be installed.

Role Variables
--------------

The main input of the role is a varible named __mydumper_backup__. This variable has an __option__ key that will mimic usage of the mydumper application's flag based system.  
For example if wanting to use the --outputdir flag (also -o), just use ___outputdir__ or ___o__.
Please look at the mydumper manual [here](https://github.com/maxbube/mydumper/blob/master/docs/mydumper_usage.rst)

Example:

New mydumper variable scheme:
```yaml
mydumper_backup:
  delete_output:                       # Will delete the output dir folder at the end of the script when true, default is false
  option:                              # Command line options or flags, underscores are converted into dashes
    __outputdir: /path/to/output/dir   # required, strongly suggest a iso8601 timestamp if S3 storage is desired
    __compress:                        # will set the compress flag, do not add a value.
    _S: 1234                           # Options are case sensitive
    _h: mysql.db.com                   # Shortname flags work too, note the single underscore.
    __regex: '"^/"'                    # Regex requires single and double quotes
    __tables_list: tb1,tb2,tb3         # This input reqires a commma seperated list
  s3:                                # Defining this key will trigger upload to S3, either path or s3 must be specified
    bucket:                          # S3 Bucket
    key_prefix:                      # Path to the folder, note that the outputdir will be appended to this prefix automatically
    region:                          # Region the bucket is in
```


```yaml
mydumper_backups:
  - __outputdir: /path/to/output/dir   # required, strongly suggest a iso8601 timestamp if S3 storage is desired
    __compress:                        # will set the compress flag, do not add a value.
    _S: 1234                           # Flags are case sensitive
    _h: mysql.db.com                   # Shortname flags work too, note the single underscore.
    __regex: '"^/"'                    # Regex need single and double quotes
    __tables_list: tb1,tb2,tb3         # This input needs a commma seperate list
    index_outputdir: false             # This will add a '-{{loop index }}' number to the outputdir, useful for keeping multi dumps seperate from each other.
    delete_local: false                # This bool will trigger the backup to be deleted at the end of the process, useful if you only want to store in s3
    s3:                                # Defining this key will trigger upload to S3
      bucket:                          # S3 Bucket
      key_prefix:                      # Path to the folder, note that the outputdir will be appended to this prefix automatically
      region:                          # Region the bucket is in
```

Dependencies
------------

I have a Redhat/Centos role that will install mydumper called _emorylib_install_mydumper_

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
