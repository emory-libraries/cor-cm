emorylib_update_chrony
=========

Updates chrony with the proper AWS server, then restarts the service

Requirements
------------

None

Role Variables
--------------

```yaml
chrony_path: /etc/chrony.conf

chrony_aws_server: server 169.254.169.123 prefer iburst minpoll 4 maxpoll 4

chrony_service_name: chronyd

chrony_clean_servers: yes                                                       # This will delete all the servers in the chrony conf before adding the AWS server

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

Solomon Hilliard for Emory Libraries
