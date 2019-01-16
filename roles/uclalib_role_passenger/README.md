uclalib_role_passenger
=========

Ansible role to install the Passenger Ruby gem and Passenger Apache HTTPD module on a RHEL7 system.

Requirements
------------

This role assumes Apache HTTPD and Ruby are already installed on the system.

Role Variables
--------------

* `passenger_version` - defines the version of Passenger to install on the system

Dependencies
------------

* uclalib_role_apache

* uclalib_role_ruby

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: uclalib_role_passenger, passenger_version: '5.3.3' }
