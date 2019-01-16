uclalib_role_nodejs
=========

Ansible role to install Nodejs on a RHEL7 system.

Requirements
------------

None.

Role Variables
--------------

* `nodejs_version` - defines the version of nodejs to install

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_nodejs, nodejs_version: '8' }
