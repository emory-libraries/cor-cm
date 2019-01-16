uclalib_role_samvera_basepackages
=========

Ansible role to install the necessary set of base packages to begin a Samvera installation

Requirements
------------

It is expected the following yum repositories are installed/configured:
  - rhel-server repos
  - epel
  - uclalib

Role Variables
--------------

* `devtools_group` - group name for the development packages

* `base_packages` - list of base packages and libraries

Dependencies
------------

Server must be connected to a RHEL server subscription, with the EPEL and uclalib repos configured.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: uclalib_role_samvera_basepackages }
