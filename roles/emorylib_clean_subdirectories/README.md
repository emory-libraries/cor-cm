emorylib_clean_subdirectories
=========

Search for and delete subdirectories older than a specified number of days. This role will only delete directories, not files. Will build a report, including when no directories are deleted, and output it to console and optionally Slack.

Requirements
------------

Must have a valid Slack token if using optional Slack feature.

Role Variables
--------------

```yaml
### Main variables
subdirectories_paths: # Required, list, --File paths to check for older subdirectories
subdirectories_age: # Required, integer --Find any subdirectories that have not been modified in these many days

### Optional Slack variables
clean_subdir_slack_token:
clean_subdir_slack_msg:
clean_subdir_slack_channel:
clean_subdir_slack_color:
clean_subdir_slack_thread_id:
clean_subdir_slack_username:
clean_subdir_slack_icon_url:
clean_subdir_slack_icon_emoji:
clean_subdir_slack_link_names:
clean_subdir_slack_parse:
```

Example Playbook
----------------

```yaml

  - hosts: server
    vars:
      subdirectories_paths: 
        - /path/to/first/dir
        - /path/to/second/dir
      subdirectories_age: 3
    tasks:
      - include_role: emorylib_clean_subdirectories
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
