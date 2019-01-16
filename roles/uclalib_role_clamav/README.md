UCLALib Ansible Role: ClamAV
=========

Installs the ClamAV Antivirus Software and the ClamAV daemon

Requirements
------------

Only supports RHEL/CentOS-family servers at this time.

Role Variables
--------------

None.

Dependencies
------------

Requires the EPEL repository be installed and enabled via `uclalib_role_epel`

Example Playbook
----------------

```
---
    - name: uclalib_clamav_app.yml
      sudo: true
      hosts: servers

    roles:
      - { role: uclalib_role_clamav }

```
