uclalib_role_epel [![Build Status](https://travis-ci.org/UCLALibrary/uclalib_role_epel.svg?branch=master)](https://travis-ci.org/UCLALibrary/uclalib_role_epel)
=========

Ansible role to install the Extra Packages for Enterprise Linux (EPEL) yum repository

Role Variables
--------------

Default variables for this role that can be overridden if needed:

* `epel_repo_url` - defines the URL where the EPEL rpm file is downloaded

* `epel_repo_file` - defines the file system path where the epel repo is configured


Example Playbook
----------------

A simple example playbook that overrides a default variable:

    ---

    - hosts: all

    # Optionally, use a different EPEL repository location
    - epel_url: https://mirrors.kernel.org/fedora-epel/6/x86_64/epel-release-6-8.noarch.rpm

    roles:
      - { role: uclalib_role_epel }

License
-------

BSD 3-Clause

Author Information
------------------

For more information about this role contact its author, [Anthony Vuong](https://github.com/avu0ng).
