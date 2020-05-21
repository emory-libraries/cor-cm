Role Name
=========

This role will perform a mysql dump using the 3rd party tool [mydumper](https://github.com/maxbube/mydumper/). The backup folder will be pushed to s3 and/or a path on the file system.

Requirements
------------

Mydumper must be installed.

Role Variables
--------------

The main input of the role is a varible named __mydumper_backup__. This variable has an __option__ key that will mimic usage of the mydumper application's flag based system.  
For example if wanting to use the --outputdir flag (also -o), just use ___outputdir__ or ___o__.
Please look at the mydumper manual [here](https://github.com/maxbube/mydumper/blob/master/docs/mydumper_usage.rst)

The second variable is called __mydumper_backup_slack__. This variable mimics the Ansible Slack module, with the same options. There is some "default" functionality built in with msg and attachments. These can be overridden by defining msg and attachments in the __mydumper_backup_slack__ variable.

Example:

```yaml
mydumper_backup:
  delete_output: no                    # Will delete the output dir folder at the end of the script when true, default is false
  notify_slack: no                     # Will output a report of the backup to slack. Requires mydumper_backup_slack to be set
  option:                              # Command line options or flags, underscores are converted into dashes
    __outputdir: /path/to/output/dir   # required, strongly suggest a iso8601 timestamp if S3 storage is desired
    __compress:                        # will set the compress flag, do not add a value.
    _S: 1234                           # Options are case sensitive
    _h: mysql.db.com                   # Shortname flags work too, note the single underscore.
    __regex: '"^/"'                    # Regex requires single and double quotes
    __tables_list: tb1,tb2,tb3         # This input reqires a commma seperated list
  s3:                                  # Defining this key will trigger upload to S3, either path or s3 must be specified
    bucket:                            # S3 Bucket
    key_prefix:                        # Path to the folder, note that the outputdir will be appended to this prefix automatically
    region:                            # Region the bucket is in

mydumper_backup_slack:
  token: undefined # required. Slack integration token.  
  username: Ansible # not required. This is the sender of the message.
  domain: undefined # not required. Slack (sub)domain for your environment without protocol. (i.e. C(example.slack.com)) In 1.8 and beyond, this is deprecated and may be ignored.  See token documentation for information.
  attachments: undefined # not required. Define a list of attachments. This list mirrors the Slack JSON API.,For more information, see also in the (U(https://api.slack.com/docs/attachments)).
  color: normal # not required. choices: normal;good;warning;danger. Allow text to use default colors - use the default of 'normal' to not send a custom color bar at the start of the message
  icon_url: undefined # not required. Url for the message sender's icon (default C(https://www.ansible.com/favicon.ico))
  parse: full # not required. choices: full;none. Setting for the message parser at Slack
  icon_emoji: undefined # not required. Emoji for the message sender. See Slack documentation for options. (if I(icon_emoji) is set, I(icon_url) will not be used)
  link_names: 1 # not required. choices: 1;0. Automatically create links for channels and usernames in I(msg).
  msg: undefined # not required. Message to send. Note that the module does not handle escaping characters. Plain-text angle brackets and ampersands should be converted to HTML entities (e.g. & to &amp;) before sending. See Slack's documentation (U(https://api.slack.com/docs/message-formatting)) for more.
  validate_certs: yes # not required. If C(no), SSL certificates will not be validated. This should only be used on personally controlled sites using self-signed certificates.
  channel: undefined # not required. Channel to send the message to. If absent, the message goes to the channel selected for the I(token).
```

Dependencies
------------

Optional role that will install mydumper:

__emorylib_install_mydumper__

Example Playbook
----------------

```yaml
- hosts: host_with_db_to_dump
  vars:
    mydumper_backup:
      delete_output: yes
      notify_slack: yes
      option:
        _o: '/tmp/{{ ansible_date_time.iso8601_micro }}'
        _h: external_db_host_address
        _B: db_name
        _u: db_username
        _p: db_password
        __lock_all_tables:                                       # Needed option for dumping RDS instances
        _t: 8
        _c:
      s3:
        bucket: s3_bucket
        key_prefix: path/to/folder
        region: us-east-1
    mydumper_backup_slack:
      token: slack/token/here
      channel: '#channelname'
```

License
-------

BSD

Author Information
------------------

Author: Solomon Hilliard for Emory Libraries
