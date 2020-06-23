emorylib_update_chrony
=========

Updates chrony with the proper AWS server, then restarts the service

Requirements
------------

None

Role Variables
--------------

```yaml
chrony_path:

chrony_aws_server:

chrony_service_name:

chrony_delete_other_servers:

```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: all
  tasks:
    - include_role:
        name: emorylib_update_chrony
      apply:
        become: yes
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
