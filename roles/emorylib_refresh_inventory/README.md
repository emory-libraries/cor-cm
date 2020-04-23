emorylib_refresh_inventory
=========

This role will do an intelligent inventory refresh. It will detect whether you are running in Ansible Core or Ansible Tower. If running in Ansible Tower it will detect a job id and refresh the inventory for that job.  

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

```yaml
tower_username: # User name for Ansible Tower, no default
tower_password: # Password for Ansible Tower, no default

tower_url: # Url where the Tower requests are being sent, default is '{{ ansible_facts.env.AWX_HOST | d () }}'
tower_refresh_retries: # Number of retries (in seconds) the role will use when checking whether the inventory refresh is completed, default is 100
tower_refresh_delay: # Delay between retries, default is 5 seconds
```

Dependencies
------------

No dependancies, this role should work in either Ansible Core or Ansible Tower.

Example Playbook
----------------

```yaml
- hosts: localhost
  roles:
    - emorylib_refresh_inventory
```

License
-------

BSD

Author Information
------------------

Solomon Hilliard
