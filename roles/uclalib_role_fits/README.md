uclalib_role_fits
=========

Ansible role for installing the Harvard File Information Tool Set (FITS) package onto a RHEL system.

Requirements
------------

Requires Java already installed on the system.

Role Variables
--------------

* `fits_version` - defines the version of FITS to download/install on the system
* `fits_tika_default_exclude_exts` - defines a comma separated list of the default file extensions the Tika library should exclude
* `fits_tika_additional_exclude_exts` - defines a comma separated list of additional file extension the Tika library should exclude

Dependencies
------------

Any java installation should work, but this role was tested against:

* `uclalib_role_java` (provides the Oracle version of Java)

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```
- name: uclalib_fits.yml
  become: yes
  become_method: sudo
  hosts: node1

  roles:
    - { role: uclalib_role_fits, fits_version: '1.3.0' }
```
