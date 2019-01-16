uclalib_role_redis
=========

Ansible role to install Redis on a RHEL7 system.

Requirements
------------

The system must have the EPEL repository configured.

Role Variables
--------------

None.

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_redis }
