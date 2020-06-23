emorylib_n2cw_custom_plugin
=========

Install nagios, n2cw (via pip) and optionally, custom nagios checks.

Requirements
------------

Pip must be installed, if running in AWS, the proper rhui servers must be enabled

Role Variables
--------------

```yaml
# Controls the name of the nagios package
nagios_plugin_package_name: nagios-plugins-all

# List of optional plugins to download, if skipping this step is desired, set n2cw_download_custom_plugins to false

n2cw_download_custom_plugins:
  - dest: /usr/lib64/nagios/plugins/check_mountpoints
    url: https://raw.githubusercontent.com/echocat/nagios-plugin-check_mountpoints/master/check_mountpoints.sh
  - dest: /usr/lib64/nagios/plugins/check_jmx
    url: https://raw.githubusercontent.com/atamariya/nagios-check-jmx/master/nagios/plugin/check_jmx
  - dest: /usr/lib64/nagios/plugins/jmxquery.jar
    url: https://raw.githubusercontent.com/atamariya/nagios-check-jmx/master/nagios/plugin/jmxquery.jar

# Override the pip install settings with this variable, copy of the pip module with a few defaults:
n2cw_pip:
  state: present
  name: n2cw
```

Example Playbook
----------------

```yaml
# Basic Playbook, with default list of custom plugins
- hosts: all
  tasks:
    - include_role:
        name: emorylib_n2cw_custom_plugin
      apply:
        become: yes

# Add additional plugins to default list
- hosts: all
  vars:
    n2cw_download_custom_plugins: '{{ n2cw_download_custom_plugins + additional_plugins }}'
    additional_plugins:
      - dest: /path/to/nagios/lib/folder
        url: http://download_url_here.com
  tasks:
    - include_role:
        name: emorylib_n2cw_custom_plugin
      apply:
        become: yes

# Skip downloading extra plugins, change some pip settings
- hosts: all
  vars:
    n2cw_download_custom_plugins: false
    n2cw_pip:
      umask: '0022'
  tasks:
    - include_role:
        name: emorylib_n2cw_custom_plugin
      apply:
        become: yes
```

License
-------

BSD

Author Information
------------------

Solomon Hilliard for Emory Libraries
