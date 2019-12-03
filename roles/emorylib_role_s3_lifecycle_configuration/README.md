Role Name
=========

Control AWS Lifecycle policies from with Ansible. Use either JSON or YAML for configuration.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

Primary varibles are the configation for the lifecycle polcies. Example of Lifecycle policies in yaml are below.
```yaml
# Specifying a filter
cli_input_json:
  Rules:
    - ID: ExampleRule
      Filter:
        Prefix: documents/
      Status: Enabled
      Transitions:
        - Days: 365
          StorageClass: GLACIER
      Expiration:
        Days: 3650
        
# Tagging Example
cli_input_json:
  Rules:
    - ID: id-1
      Filter:
        And:
          Prefix: myprefix
          Tags:
          - Value: mytagvalue1
            Key: mytagkey1
          - Value: mytagvalue2
            Key: mytagkey2
      Status: Enabled
      Expiration:
        Days: 1
```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
# emorylib_role_s3_lifecycle_polcies
