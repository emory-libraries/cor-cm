ClamAV ansible role
=========

Installs ClamAV on RedHat/CentOS and Debian/Ubuntu Linux servers :

* On Access scan and access prevention can be activated
* Periodic scan can be activated (clamscan or faster clamdscan when clamd daemon runs as the root user)
* Customizable script when detecting virus : only for periodic or manual scans (not for On Access scan)
  see https://bbs.archlinux.org/viewtopic.php?id=237489 and ClamAV source code to check status on this
* Freshclam can use an authenticated proxy to update virus databases

Lots of templates and variables but feel free to add/request some more if needed

Requirements
------------

EPEL repositories muse be enabled on RHEL/CentOS (not adding dependency because you may already have something to enable EPEL)

Role Variables
--------------

Available variables are listed below, see default values in `defaults/main.yml`:
You can override them in host/group files
See OS-specific variable files in var/

* Freschlam database update
	* Configure optional freshclam http proxy settings using **freshclam\_proxy.XXXX** variables : (each one is optional) :  

	        freshclam_proxy:
              hostname: "proxy.mydomain.org"
              port: "8080"
              username: "interneuser"
              password: "internetP4ssW0rD"
      
* ClamAV daemon service status : started or not and/or enabled or not :
    * **clamav\_daemon\_state**: started
    * **clamav\_daemon\_enabled**: true

* ClamAV daemon temporary directory (must not be included as "On Access", see https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=904111  
 *This role will check and prevent that*  
 **clamav\_daemon\_temporary\_directory**: "/var/tmp"    

* ClamAV clamdscan max sizes : (optional) (note : on access scanning has its own values)
    note : not overwriting ClamAV default values in this role
    * **clamav_daemon_max_scan_size**: "100M" (default ClamAV value)
    * **clamav_daemon_max_file_size**: "25M"  (default ClamAV value)

* ClamAV freshclam service status
    * **clamav\_freshclam\_daemon\_state**: started
    * **clamav\_freshclam\_daemon\_enabled**: true

* ClamAV freshclam configuration file
	* **clamav\_freshclam\_daemon\_config\_path**: /etc/freshclam.conf

* If you do want to change default permissions on ClamAV daemon socket to switch the allowed group to use it
 *Beware that any user user within this group can talk to the clamav daemon, even to shut it down -> DoS*
	* **clamav_daemon_socket_group**: virusgroup

* What to do when a virus if found :
	* command to launch when virus is found :  
      **clamav\_daemon\_virus\_event\_command** : 'echo $(date) - ${CLAM\_VIRUSEVENT\_VIRUSNAME}: ${CLAM\_VIRUSEVENT\_FILENAME} | wall'

    * **OR**  External script to launch when virus is found : 
        * use external script instead of inline command :  
          **clamav\_daemon\_virus\_event\_use\_script**: true    
          *this script deployed from template clamav\_virus\_alert.sh.j2 to $***clamav\_daemon\_virus\_event\_script\_name** *if* $**clamav\_daemon\_virus\_event\_ansible\_deploy\_script** *is true*

        * default name of the external script :  
          **clamav\_daemon\_virus\_event\_script\_name**: "/usr/local/sbin/clamav\_virus\_alert.sh"

* On Access scan features : (defining clamav\_onaccess\_include\_paths or clamav\_onaccess\_mount\_paths wil enable On Access scanning, and thus clamd will run as the root user)
	* Paths to be included to On Access scan :  
	  **clamav\_onaccess\_include\_paths** (include path list)
 
	* Mount paths to be included to On Access scan :  
	  **clamav\_onaccess\_mount\_paths** (mount path list)

	* Paths to be excluded from On Access scan  
	  **clamav\_onaccess\_exclude\_paths** (exclude path list)

    **note : OnAccessIncludePath option will be ignored if OnAccessMountPath is enabled (extract from clamd documentation)**

* infected files access prevention : user will get a permission denied when trying to access infected file
    **clamav\_daemon\_access\_prevention**: no

* Periodic scan features :
	* Enable or not periodic scan  
	  **clamav\_periodic\_scan\_enabled**: true
    * Periodicity of this periodic scan : choose from "hourly", "daily", "weekly" or "monthly"  
      **clamav\_periodic\_scan\_periodicity**: "weekly"  
    * Which script to launch ? :  
        **clamav\_periodic\_scan\_script**: "/usr/local/sbin/clamav\_periodic\_scan.sh"
    * Periodic scan logfile :  
        **clamav\_periodic\_scan\_logfile**: "/var/log/clamav\_periodic\_scan.log"  
    * Name of the logrotate donfigfile for the above logfile :  
        **clamav\_periodic\_scan\_logrotate\_file**: "/etc/logrotate.d/clamav-periodic-scan"
    * Directories to exclude from periodic scan (clamscan): (default values) :  
        **clamav\_periodic\_scan\_exclusions**:  
  
            clamav_periodic_scan_exclusions:
              - /dev
              - /proc
              - /run
              - /sys

    * Same thing in regexp format for clamd configuration (and periodic clamdscan): (default values)
        **clamav\_daemon\_scan\_exclusion\_regexps**:
  
            clamav_periodic_scan_exclusion_regexps:
              - "^/dev/"  
              - "^/proc/"  
              - "^/run/"  
              - "^/sys/"  
              - "^/var/lib/sss/pipes/"  
              - "^{{ clamav_daemon_temporary_directory }}/"  
              # access denied on /var/log/audit/audit.log* anyway :p (and the following files/drectories) :  
              - "^/var/log/audit/"  
              - "^/etc/audit/"  
              - "^/etc/selinux/"  
              - "^/etc/(g)?shadow(-)?$"  
              - "^/etc/krb5.keytab$"  
              - "^/etc/security/opasswd$"

    * Email alerting when using built-in alerting script template :
        * From: using "Fancy Name &lt;email@ddre.ss&gt;"  
        **clamav\_daemon\_virus\_event\_mail\_from**: "Virus Alert &lt;noreply@mydomain.org&gt;"
        * To: using adressses list :  
        **clamav\_daemon\_virus\_event\_mail\_to**:  

                clamav_daemon_virus_event_mail_to:
                  - "root@mydomain.org"
                  - "security@mydomain.org"

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: myserver.mydomain.org
      become: true

      roles:
        - role: clamav
          freshclam_proxy:
            # note that proxy settings are here inherited :
            hostname: "{{ global_proxy.hostname }}"
            port: "{{ global_proxy.port }}"
            username: "{{ global_proxy.username }}"
            password: "{{ global_proxy.password }}"
          clamav_daemon_virus_event_mail_from: "Virus Alert <noreply@mydomain.org>"
          # Email alert "To:" using list of adressses :
          clamav_daemon_virus_event_mail_to:
            - "virusadmin@mydomain.org"
            - "virusmaster@mydomain.org"
          # enable On Access scanning on the following paths :
          clamav_onaccess_include_paths:
            - "/root"
            - "/home"
            - "/tmp"
          # prevent infected virus files access :
          clamav_daemon_access_prevention: true
          # clamdscan max size parameters :
          clamav_daemon_max_scan_size: "950M"
          clamav_daemon_max_file_size: "950M"


More complex exemple adding some user allowed to launch clamdscan

    - hosts: myserver.mydomain.org
      become: true

      roles:
        - role: clamav
          freshclam_proxy:
            # note that proxy settings are here inherited :
            hostname: "{{ global_proxy.hostname |default() }}"
            port: "{{ global_proxy.port |default() }}"
            username: "{{ global_proxy.username |default() }}"
            password: "{{ global_proxy.password |default() }}"
          clamav_daemon_virus_event_mail_from: "Virus Alert <noreply@mydomain.org>"
          # Email alert "To:" using list of adressses :
          clamav_daemon_virus_event_mail_to:
            - "virusadmin@mydomain.org"
            - "virusmaster@mydomain.org"
          # enable On Access scanning on the following paths :
          clamav_onaccess_include_paths:
            - "/root"
            - "/home"
            - "/tmp"
          # prevent infected virus files access :
          clamav_daemon_access_prevention: true
          # clamdscan max size parameters :
          clamav_daemon_max_scan_size: "950M"
          clamav_daemon_max_file_size: "950M"

      tasks:
        # add some user and allow him to run clamdscan
        # group must exist before we can create user
        - name: "Create group {{ username }}"
          group:
            name: "{{ username }}"

        - name: "Create user {{ username }}"
          user:
            name: "{{ username }}"
            group:  "{{ username }}"
            createhome: yes
            shell: /bin/bash
            groups: "{{ clamav_daemon_socket_group }}"
            append: yes


License
-------

WTFPL

Author Information
------------------

This role was created in 2019 by [Jérôme Drouet](https://github.com/jeromedrouet/)
