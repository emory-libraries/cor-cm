# UCLALib Ansible Role: Solr

Ansible role to install Apache Solr 7 on RHEL servers and set-up Solr cores

## Requirements

This role is capable of deploying Solr in the following configurations:

* Standalone: a single Solr instance

It is assumed that Java and HTTPD are installed on the host system.

If you have project-specific Solr configuration files, they should be placed in the `files` role directory inside a sub-directory named `projname_solr-conf`, where `projname` is an identifier you choose to represent the custom config files for this project.

## Variables

* `solr_version` - defines the version of Solr to use (e.g. 4.2.0, etc.)
* `solr_url` - defines the URL to download Solr
* `solr_user` - defines the user to own Solr files and run the Solr process
* `solr_service_name` - defines the name of the Solr process
* `solr_install_dir` - defines the top-level directory where Solr will be installed
* `solr_base_dir` - defines the full path where Solr is installed
* `solr_data_dir` - defines the full path where the Solr indexes/cores will reside
* `solr_port` - defines the network port the Solr process will bind to
* `solr_config_file` - defines the path to the Solr installation configuration file
* `solr_java_min_mem` - defines the Solr process minimum memory allocation
* `solr_java_max_mem` - defines the Solr process maximum memory allocation
* `solr_replication_node` - defines the Solr replication status for this system (default is `none` - options are `master`, `slave`, `none`)
* `solr_replication_master_url` - defines the URL path to the Solr Master node (only necessary if `solr_replication_node` set to `slave`)
* `solr_cores` - the dictionary list of all Solr cores to be used in this instance
  * `ident` - defines the names of the cores to configure within this Solr instance
  * `type` - defines the project name used to reference which custom solr configs to use (e.g. default, drupal, hyrax etc).
    * the `type` variable is used in conjunction with application specific solr configuration files. You will need to manually put the appropriate files in the `files` directory for this role.
      * the naming convention for solr configuration files is: `projname_solr-conf`
      * for example, if your `type` is set to drupal, then the custom solr config file dir should be named: `drupal_solr-conf`

## Solr Download URL Note

The default value for the `solr_url` variable is:

`http://archive.apache.org/dist/lucene/solr/{{ solr_version }}/solr-{{ solr_version }}.tgz`

If you are affiliated with UCLA, you have the option of overriding this default url value with:

`http://pkgs.library.ucla.edu/apache-solr/solr-{{ solr_version }}.tgz`

Versions of Solr available via the UCLA URL are:

* `7.4.0`

## Dependencies

* uclalib_role_rhel7repos
* uclalib_role_java
* uclalib_role_apache

## Sample Variable Definition Formats

#### Standalone Solr Instance
```
solr_cores:
  - ident: core1
    type: default
  - ident: core2
    type: drupal
  - ident: core3
    type: hyrax
```

The variable definitions should be placed in the playbook under the `vars` statement.
Example:
```
---
- name: uclalib_solr.yml
  sudo: true
  hosts: test
  vars:
    solr_cores:
      - ident: core1
        type: default
      - ident: core2
        type: drupal
      - ident: core3
        type: hyrax

    roles:
      - { role: uclalib_role_java, oracle_java_version: '1.8.0_181' }
      - { role: uclalib_role_apache }
      - { role: uclalib_role_solr, solr_version: '7.4.0' }
```
