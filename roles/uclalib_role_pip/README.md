uclalib_role_pip
=========

An ansible role that installs the python pip package manager onto a RHEL/CentOS system

Requirements
------------

None.

Role Variables
--------------

* `python_version` - defines the version of python to install - valid options are:
    * `default` - use the default version of python packaged with the OS
    * `34`, `36`, etc - use a version of python you know is available in the EPEL repo


* `python_base_packages` - defines a list of python packages to install on the system

* `pip_install_packages` - defines an optional set of pip packages to install on the system

* `use_virtualenv` - defines if python should be installed in a virtualenv (default is `no`)

* `virtualenv_path` - if `use_virtualenv` is `yes` this must be configured to define the location of the virtualenv directory

Dependencies
------------

When installing the default version of python - no dependencies.

When installing a version of python that does not come with the RHEL OS, ensure the EPEL repository is configured.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_pip }
