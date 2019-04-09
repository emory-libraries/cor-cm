wm_role_Shibboleth_SP
=========

This role installs and configures shibboleth on RedHat and CentOS.

Required Role Variables
--------------

* `shib_vhost_path` - This variable is the pathway the role will look for when making modifications to shibd.conf apache file
* `sp_home` - The directory shibboleth is installed to
* `idp_attribute_map_src` - Source for the IDP's attribute map
* `idp_environments` - List of IDP environments
	`- name` - Name of environment, will be used by another variable called `idp_name`
	`url`  - URL of the IDP

* `support_contact` - Shibboleth xml file showing where to get support from
* `remote__user` - Shibboleth xml file REMOTE_USER

Optional Role Variables
--------------
* `get_metadata` - Dictionary variable that controls how and if the metadata of the target is downloaded to the host
    `download` Boolean that controls if metadata is download to host
    `url` String that targets where the metadata will be downloaded from
    `dest` Filepath of XML metadata
