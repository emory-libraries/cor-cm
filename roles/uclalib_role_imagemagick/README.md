UCLALib Ansible Role: ImageMagick
=========

Installs the ImageMagick image manipulation library and utility scripts.

Requirements
------------

Only supports RHEL/CentOS-family servers at this time.

Role Variables
--------------

None.

Dependencies
------------

None.

Example Playbook
----------------

```
---
    - name: uclalib_imagemagick_app.yml
      sudo: true
      hosts: servers

    roles:
      - { role: uclalib_role_imagemagick }

```
