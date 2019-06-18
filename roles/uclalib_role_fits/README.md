uclalib_role_fits
=========

Ansible role for installing the Harvard [File Information Tool Set (FITS)](https://projects.iq.harvard.edu/fits) package onto a RHEL system.

Optionally install [FITS as a web service](https://github.com/harvard-lts/FITSservlet) in an existing Tomcat environment.

Requirements
------------

Requires Java already installed on the system.

For the installing as a web service - also requires Tomcat to already be installed on the system.

Role Variables
--------------

* `fits_version` - defines the version of FITS to download/install on the system

* `fits_tika_default_exclude_exts` - defines a comma separated list of the default file extensions the Tika library should exclude

* `fits_tika_additional_exclude_exts` - defines a comma separated list of additional file extension the Tika library should exclude

* `fits_run_as_webservice` - defines if the FITS web service should also be installed (default is `no`)

* `fits_webservice_war_version` - defines the version of the FITS web service war file to be installed (only used if `fits_run_as_webservice` is `yes`)

FITS Download URL Note
----------------------

The default value for the `fits_download_url` variable is:

`http://projects.iq.harvard.edu/files/fits/files/fits-{{ fits_version }}.zip`

If you are affiliated with UCLA, you have the option of overriding this default url value with:

`http://pkgs.library.ucla.edu/fits/fits-{{ fits_version }}.zip`

Versions of FITS available via the UCLA URL are:

* `1.3.0`

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
