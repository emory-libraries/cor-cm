emorylib_n2cw_cron
=========

This role will install nagios and its plugins, and pip install n2cw, which allows nagios plugins to report to cloudwatch. The role will be as flexible as possible while simplifying the cron module of ansible.

Requirements
------------

Role is intended for Red Hat 7 Enterprise Linux, but should work on other versions of linux.

Role Variables
--------------

```yaml
n2cw_cron_envs:                                         # Optional list of cron environment variables to manage.
  - name: APP_HOME                                      # Required, and must be unique
    value: /opt/whatever                                # Required, is the value or 'job' of the environment variable
    state: present                                      # Optional, defaults to present
    insertafter:                                        # Optional, cron.insertafter value
    insertbefore:                                       # Optional, cron.insertbefore value
## Options that act as defaults for the n2cw_cron_jobs variable, that CANNOT be overridden by indiviual items inside the list.
n2cw_cron_user:                                         # Optional, sets the cron.user value  
n2cw_cron_file:                                         # Optional, sets the cron.file value  
n2cw_cron_backup: no                                    # Optional, sets the cron.backup value, defaults to no
## Options that act as defaults for the n2cw_cron_jobs, that CAN be overridden by indiviual items inside the list.
  ## Cron Options
n2cw_cron_day:
n2cw_cron_weekday:
n2cw_cron_hour:
n2cw_cron_minute:
n2cw_cron_second:
  ## n2cw Options
n2cw_namespace:                                         # Required, sets the namespace for cloudwatch
n2cw_plugin_path:                                       # Required, sets the path for nagios plugins
n2cw_job:
## Main Input Variable
n2cw_cron_jobs:
  - name:                                               # Required, and must be unique. Must be the plugin name if plugin is not specified!
    disabled:                                           # Optional, cron.disabled
    plugin:                                             # Optional, the plugin name, higher presidence than n2cw_cron_jobs.name
    state: present                                      # Optional, cron.state, defaults to present
    options:                                            # Optional, list of plugin options, joined by ' -', do not put dash inside value
    namespace:                                          # Optional, higher presidence than n2cw_namespace
    plugin_path:                                        # Optional, higher presidence than n2cw_plugin_path
    job:                                                # Optional, higher presidence than n2cw_job
```

Dependencies
------------

Pip must be installed.

EPEL must be enabled.

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
