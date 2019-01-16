uclalib_role_libreoffice
=========

Ansible role to install LibreOffice on a RHEL7/CentOS7 system.

Requirements
------------

Java must already be installed on the system for LibreOffice to function.

Role Variables
--------------

* `libreoffice_version` - defines the version of LibreOffice to install

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_libreoffice, libreoffice_version: '5.4.7' }
