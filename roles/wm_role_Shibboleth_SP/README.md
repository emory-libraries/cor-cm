Role Name
=========
wm_role_Shibboleth_SP

Install Shibboleth SP for RedHat7, CentOS7, Redhat6 or CentOS6.

Requirements
------------

NONE.

Role Variables
--------------

SP_EntityID:  "https://{{ ansible_hostname }}"

SP_Home: /etc/shibboleth
Shib_Repo:  http://download.opensuse.org/repositories/security://shibboleth/CentOS_7/security:shibboleth.repo
# RHEL7 or CentOS7
RHEL7:  http://download.opensuse.org/repositories/security://shibboleth/CentOS_7/security:shibboleth.repo
CentOS7:  http://download.opensuse.org/repositories/security://shibboleth/CentOS_7/security:shibboleth.repo
# RHEL6 or CentOS6
RHEL6:  http://download.opensuse.org/repositories/security://shibboleth/RHEL_6/security:shibboleth.repo
CentOS6:  http://download.opensuse.org/repositories/security://shibboleth/CentOS_CentOS-6/security:shibboleth.repo

#  Defalt Install of SP
IdP_Default_EntityID:  "https://idp.example.org/idp/shibboleth"
SP_Default_EntityID:  "https://sp.example.org/shibboleth"

#  PROD IdP
Prod_IdP_EntityID:  "https://login.emory.edu/idp/shibboleth"
Prod_IdP_MetadataURL:  "https://login.emory.edu/idp/shibboleth"
Prod_IdP_MetadataFile:  "/etc/shibboleth/PROD-idp-metadata.xml"

#  QA IdP
Qa_IdP_EntityID:  "https://login.emory.edu:443/idp/shibboleth"
Qa_IdP_MetadataURL:  "https://login.emory.edu:4443/idp/shibboleth"
Qa_IdP_MetadataFile: "/etc/shibboleth/QA-idp-metadata.xml"

#  DEV IdP
Dev_IdP_EntityID:  "https://login.emory.edu:444/idp/shibboleth"
Dev_IdP_MetadataURL:  "https://login.emory.edu:4444/idp/shibboleth"
Dev_IdP_MetadataFile: "/etc/shibboleth/DEV-idp-metadata.xml"

#  Same for PROD, QA and DEV
Emory_AttributeMapUrl:  "https://login.emory.edu/client-config/attribute-map.xml"
Emory_AttributeMap:  "/etc/shibboleth/Emory_attribute-map.xml"
AttributeMap:  "/etc/shibboleth/attribute-map.xml"
Emory_SPConfFiles:  "https://secure.web.emory.edu/advtest/SpConfFiles.zip"
YumRepo: "/etc/yum.repos.d/"


PROD_IdP_MetadataProvider:  "\n\t<MetadataProvider type=\"Chaining\">\n
                         \t\t<MetadataProvider type=\"XML\" url=\"https://login.emory.edu/idp/shibboleth\"\n
                         \t\t\tbackingFilePath=\"PROD-idp-metadata.xml\" reloadInterval=\"28800\">\n
                         \t\t</MetadataProvider>\n
                         \t</MetadataProvider>\n"


QA_IdP_MetadataProvider:  "\n\t<MetadataProvider type=\"Chaining\">\n
                         \t\t<MetadataProvider type=\"XML\" url=\"https://login.emory.edu:4443/idp/shibboleth\"\n
                         \t\t\tbackingFilePath=\"PROD-idp-metadata.xml\" reloadInterval=\"28800\">\n
                         \t\t</MetadataProvider>\n
                         \t</MetadataProvider>\n"


DEV_IdP__MetadataProvider:  "\n\t<MetadataProvider type=\"Chaining\">\n
                         \t\t<MetadataProvider type=\"XML\" url=\"https://login.emory.edu:4444/idp/shibboleth\"\n
                         \t\t\tbackingFilePath=\"PROD-idp-metadata.xml\" reloadInterval=\"28800\">\n
                         \t\t</MetadataProvider>\n
                         \t</MetadataProvider>\n"


#  Playbook Defaults
State: "present"
IdP_EntityID: "{{ Prod_IdP_EntityID }}" 
IdP_MetadataURL:  "{{ Prod_IdP_MetadataURL }}"
IdP_MetadataProvider: "{{ PROD_IdP_MetadataProvider }}"
IdP_MetadataFile: "{{ Prod_IdP_Metadata_File }}"
RemoteUser: "uid eppn subject-id pairwise-id persistent-id"
# This should be changed to the email address of the actual support contact.
SupportContact: "XXXXX@emory.edu"


Dependencies
------------

NONE.

Example Playbook
----------------

---

- hosts: gerrywebservers
  gather_facts: True
  become: yes
  serial: 1
   
  vars:
        State: "present"
        #State: "latest"
        SP_EntityID: "https://foo.emory.edu"
        RemoteUser: "uid eppn subject-id pairwise-id persistent-id"
        SupportContact: "jsmith@emory.edu"

  #  For PROD IdP (the defaults)
        IdP_EntityID: "{{ Prod_IdP_EntityID }}" 
        IdP_MetadataURL:  "{{ Prod_IdP_MetadataURL }}"
        IdP_MetadataProvider: "{{ PROD_IdP_MetadataProvider }}"
        IdP_MetadataFile: "{{ Prod_IdP_Metadata_File }}"

  #  For QA IdP 
        #IdP_EntityID: "{{ Qa_IdP_EntityID }}" 
        #IdP_MetadataURL:  "{{ Qa_IdP_MetadataURL }}"
        #IdP_MetadataProvider: "{{ Qa_IdP_MetadataProvider }}"
        #IdP_MetadataFile: "{{ Qa_IdP_Metadata_File }}"

  #  For DEV IdP 
        #IdP_EntityID: "{{ Dev_IdP_EntityID }}" 
        #IdP_MetadataURL:  "{{ Dev_IdP_MetadataURL }}"
        #IdP_MetadataProvider: "{{ Dev_IdP_MetadataProvider }}"
        #IdP_MetadataFile: "{{ Dev_IdP_Metadata_File }}"



  tasks:
  - include_role:
      name: wm_role_Shibboleth_SP




License
-------

BSD

Author Information
------------------

Gerry Hall (ghall4@emory.edu)
