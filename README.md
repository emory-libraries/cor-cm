cor-cm
=========
This reposititory is the central point of control for the COR project from Emory University Libraries. Since Emory has a cloud first policy, this repo is designed for exclusive use with Amazon Web Services(AWS).


Requirements
------------
On the host machine, please install the latest version of Ansible (preferred method is pip) and also: boto3, botocore and Jinja > 2.10

Role Variables
--------------
The role variables depend on which type of web app is being built. Blacklight apps will require the blacklight's roles, etc. They playbooks for the web apps are located in /playbooks


Example Playbook
----------------

All the playbooks have hosts: all, so please use the -l command line to limit the servers to the ones you want to be created.

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
