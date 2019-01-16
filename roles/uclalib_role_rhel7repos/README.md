uclalib_role_rhel7repos
=========

Ansible role to ensure the base set of RHEL7 yum repositories are enabled on the system.

These repositories include:

* `rhel-7-server-rpms`
* `rhel-7-server-optional-rpms`
* `rhel-7-server-supplementary-rpms`
* `rhel-7-server-extras-rpms`

Requirements
------------

The RHEL7 system must already be attached to a subscription with Red Hat.

Role Variables
--------------

None.

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: uclalib_role_rhel7repos }
