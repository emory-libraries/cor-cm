emorylib_n2cw_cron
=========

Manage a list of cron jobs using the nagios to CloudWatch (n2cw) pip module to push custom metrics to CloudWatch

Requirements
------------

Role is intended for Red Hat 7 Enterprise Linux, but should work on other versions of linux. Nagios and n2cw must be installed. Please run the [emorylib_n2cw_custom_plugin](https://github.com/emory-libraries/emorylib_n2cw_custom_plugin)

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
n2cw_cron_day: *
n2cw_cron_weekday: *
n2cw_cron_hour: *
n2cw_cron_minute: *
n2cw_cron_second: *
  ## n2cw Options
n2cw_namespace:                                         # Required, sets the namespace for cloudwatch
n2cw_plugin_path:                                       # Required, sets the path for nagios plugins
n2cw_job:                                               # Sets the cron job, has a default but can be overrridden if desired
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

emorylib_n2cw_custom_plugin

Example Playbook
----------------

```yaml
# Basic variable configuration with needed values set
- hosts: all
  vars:
    n2cw_plugin_path: /usr/lib64/nagios/plugins         # No trailing slash
    n2cw_namespace: cloudwatch_namespace
    n2cw_cron_minute: '*/5'                             # Set the default period to once every five minutes
    n2cw_cron_envs:                                     # Set the user's BASH_ENV
      - name: BASH_ENV
        value: '/home/{{ ansible_user }}/.bashrc'
    n2cw_cron_jobs:                                     # Main input variable
      - name: check_httpd_procs                         # Name your checks similar to the plugin being used
        plugin: check_procs
        options:                                        # This section will output as "-C httpd -c 1:500"
          - C httpd
          - 'c 1:500'                                   # Surround special characters with quotes
  tasks:
    - include_role:
        name: emorylib_n2cw_custom_plugin
        apply:
          become: yes
    - include_role:
        name: emorylib_n2cw_cron
## More complicated cron jobs
  vars:
    n2cw_cron_jobs:
      - name: check_zookeeper_procs
        disabled: yes                                 # Will setup the cron job but leave it commented out
        plugin: check_procs
        options:
           - a zookeeper
           - 'c 2:'
      - name: check_solr_procs
        minute: '*/1'                                 # Override default period and check every minute
        plugin: check_procs
        options:
          - a solr
          - 'c 1:'
      - name: check_efs
        plugin: check_mountpoints                     # This is a custom plugin downloaded by the emorylib_n2cw_custom_plugin role
        options:
          - A
      - name: check_httpd_procs
        namespace: 2nd_cloudwatch_namespace           # Send this check to another namespace
        plugin: check_procs
        options:
          - C httpd
          - 'c 1:500'
      - name: check_jmx_used_heap
        plugin: check_jmx                             # This is a custom plugin downloaded by the emorylib_n2cw_custom_plugin role
        options:
          - 'U service:jmx:rmi:///jndi/rmi://localhost:9004/jmxrmi'
          - 'O java.lang:type=Memory'
          - A HeapMemoryUsage
          - K used
          - I HeapMemoryUsage
          - J used
          - vvvv
          - w 805306368                               # 750 MB
          - c 1181116006                              # 1.1 GB
```

License
-------

BSD

Author Information
------------------

Solomon Hilliard for Emory Libraries
