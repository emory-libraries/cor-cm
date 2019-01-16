uclalib_role_ffmpeg
=========

Ansible role to install a static binary build of ffmpeg on a RHEL system

Requirements
------------

None.

Role Variables
--------------

* `ffmpeg_base_url` - defines the home page/base url where ffmpeg is made available - https://johnvansickle.com/ffmpeg/

* `ffmpeg_version` - defines the version of ffmpeg to obtain
  - for the latest version available use: "`release`"
  - for a specific version use the version number: "`4.0.1`"


* `ffmpeg_install_dir` - defines the directory to install ffmpeg

* `ffmpeg_user_path_dir` - define the directory to place a symblink in the user path

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_ffmpeg, ffmpeg_version: '4.0.1' }
