cor-cm
=========
This reposititory is the central point of control for the COR project from Emory University Libraries. Since Emory has a cloud first policy, this repo is designed for exclusive use with Amazon Web Services(AWS).


Requirements
------------
On the host machine, install the latest version of Ansible, boto3, botocore, and Jinja2 (preferred method is pip)

Role Variables
--------------
The role variables depend on which type of web app is being built. Blacklight apps will require the blacklight's roles, etc. They playbooks for the web apps are located in /playbooks


Example Playbook
----------------
The `site-` playbooks are the playbooks intended for creating the websites and the playbooks are hard coded with the group they're named after.
For example, site-lux is hard coded with the lux group.
To use a site- playbook
`ansible playbook playbooks/site-lux.yml` This command would create the lux group, which consists of digital.library.emory.edu and digital-test.library.emory.edu
Additionally, the --limit (-l) functionality of ansible has been preserved, if creating only digital.library.emory.edu was desired then the command would be:
`ansible playbook playbooks/site-lux.yml -l digital.library.emory.edu`

License
-------
BSD
