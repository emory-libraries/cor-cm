uclalib_role_rhelscl [![Build Status](https://travis-ci.org/UCLALibrary/uclalib_role_rhelscl.svg?branch=master)](https://travis-ci.org/UCLALibrary/uclalib_role_rhelscl)
=========

Enable the RHEL Software Collections (SCL) repository and install/enable SCL packages.

List of available packages can be found within the [Product Documentation for Red Hat Software Collections](https://access.redhat.com/documentation/en-us/red_hat_software_collections)

Requirements
------------

When run against a RHEL7 system, it must already be registered with Red Hat and attached to a valid subscription.

Role Variables
--------------

* `scl_packages` - list of SCL packages to install and whether or not to enable at log-in. By default this variable is not defined. Sample usage:
  ```
  scl_packages:
    - pkg_name: "rh-python36"
      scl_enable: "yes"
    - pkg_name: "rh-git218"
      scl_enable: "no"
    - pkg_name: " rh-ruby25"
      scl_enable: "yes"
  ```

Dependencies
------------

For RHEL7 systems:
* `uclalib_role_rhel7repos`
* `uclalib_role_epel`

For CentOS7 systems:
* `uclalib_role_epel`

Example Playbook
----------------

```
---

- name: uclalib_rhelscl.yml
  become: yes
  become_method: sudo
  hosts: all
  vars:
    scl_packages:
      - pkg_name: "rh-python36"
        scl_enable: "yes"

  roles:
    - { role: uclalib_role_rhelscl }
```
