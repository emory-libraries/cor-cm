uclalib_role_libreoffice
=========

Ansible role to install LibreOffice on a RHEL7/CentOS7 system.

Requirements
------------

Java must already be installed on the system for LibreOffice to function.

Role Variables
--------------

* `libreoffice_version` - defines the version of LibreOffice to install

LibreOffice Download URL Note
-----------------------------

The default value for the `libreoffice_download_url` variable is:

`http://download.documentfoundation.org/libreoffice/stable/{{ libreoffice_version }}/rpm/x86_64/LibreOffice_{{ libreoffice_version }}_Linux_x86-64_rpm.tar.gz`

If you are affiliated with UCLA, you have the option of overriding this default url value with:

`http://pkgs.library.ucla.edu/libreoffice/LibreOffice_{{ libreoffice_version }}_Linux_x86-64_rpm.tar.gz`

Versions of LibreOffice available via the UCLA URL are:

* `6.0.7`

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_libreoffice, libreoffice_version: '5.4.7' }
