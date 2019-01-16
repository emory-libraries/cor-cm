uclalib_role_pip
=========

An ansible role that installs the python pip package manager onto a RHEL/CentOS system

Requirements
------------

None.

Role Variables
--------------

* `python_base_packages` - defines the base set of python packages to install on the system

* `pip_install_packages` - defines an optional set of pip packages to install on the system

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_pip }
