# UCLALib Ansible Role: Fedora 4

Ansible role to install and configure the Fedora repository application on a RHEL system using MySQL for the back-end.

Deploy a bare-bones installation/configuration of a Fedora 4 repository server.

___This role supports Fedora 4.7.X releases only.___

Deployment documentation is for this version is available at [Deploying Fedora 4 Complete Guide](https://wiki.duraspace.org/x/PjdsBQ).

Official GitHub repository:
* https://github.com/fcrepo4-exts/fcrepo-webapp-plus
* https://github.com/fcrepo4/fcrepo4

## Requirements

Please take note of the following assumptions:
* the Fedora repository server uses Red Hat Enterprise Linux as the OS
* a MySQL database server is available with the database created and user account/privileges established
* project-specific variables for this role can be defined in a __vars__ file with a name following the format of `projectname_envname.yml`
    * an example vars file is available in `vars/exampleproj_test.yml`
    * this vars file will contain sensitive information and should be encrypted with ansible-vault
    * NOTE: if you choose not to use the vars file for including the variable definitions, they should be defined in the playbook file

## Role Variables

Variables that need to be defined in the **play file** or the **host inventory file** - please note these should match the naming used for the vars file:
* `project_name` - defines the name of the rails application project - there is no default value
* `env_name` - defines the name of the deploy environment (e.g. test, stage, prod) - there is no default value
* `fedora_version` - defines the version of Fedora to use (e.g. 4.0.0, 4.5.0, etc.)

Variables with default values that **do not** need to be defined in the project vars file, but can be adjusted if necessary:
* `fedora_url` - defines the URL to obtain the Fedora WAR file
* `fedora_base_dir` - defines the location of Fedora Tomcat installation directory
* `fedora_tomcat_user` - defines the tomcat user fedora will run as
* `fedora_min_mem` -  defines the fedora/tomcat minimum memory allocation
* `Fedora_max_mem` - defines the fedora/tomcat maximum memory allocation
* `fedora_data_dir` - defines the location of the Fedora data directory
* `fedora_config_dir` - defines the path to the external fedora configuration directory
* `fedora_log_dir` - defines the path to the fedora log directory

Variables that **do** need to be defined in the project vars file:
* `fedora_db_user` - defines the user fedora will use to connect to the database
* `fedora_db_password` - defines the password fedora will use to connect to the database
* `fedora_db_host` - defines the FQDN of the database server
* `fedora_db_port` - defines the network port for connecting to the database
* `fedora_db_name` - defines the name of the fedora database
* `fedora_admin_user` - defines the Fedora admin username
* `fedora_admin_password` - defines the Fedora admin password
* `fedora_server_fqdn` - defines the fully qualified domain name of the Fedora repository server

An example vars file is available as a part of this role, named `exampleproj_test.yml`

## Fedora Download URL Note

The default value for the `fedora_url` variable is:

`https://github.com/fcrepo4/fcrepo4/releases/download/fcrepo-{{ fedora_version }}/fcrepo-webapp-{{ fedora_version }}.war`

If you are affiliated with UCLA, you have the option of overriding this default url value with:

`http://pkgs.library.ucla.edu/fedora/fcrepo-webapp-{{ fedora_version }}.war`

Versions of Fedora available via the UCLA URL are:

* `4.7.5`

## Dependencies

The following roles must be run on the Fedora repository server prior to executing this role:

* uclalib_role_rhel7repos
* uclalib_role_epel
* uclalib_role_uclalibrepo
* uclalib_role_java
* uclalib_role_apache
* uclalib_role_tomcat

## Sample Variable Definition Formats

```
---

- name: uclalib_fedoratest.yml
  become: yes
  become_method: sudo
  hosts: all
  user: ansible
  vars:
    project_name: fedoraproject
    env_name: test
    tomcat_applications:
      - app_name: fedora
        shut_port: 8008
        conn_port: 8081
        rproxy_path: fcrepo

  roles:
    - { role: uclalib_role_rhel7repos }
    - { role: uclalib_role_epel }
    - { role: uclalib_role_uclalibrepo }
    - { role: uclalib_role_java, oracle_java_version: '1.8.0_181' }
    - { role: uclalib_role_apache }
    - { role: uclalib_role_tomcat }
    - { role: uclalib_role_fedora4, fedora_version: '4.7.5' }
```
