ipaserver
=========

A simple, but fairly parameterized, role for setting up a FreeIPA / Red Hat IDM server. Built for RHEL7, but supports CentOS 7 and Fedora also.

Credits
-------
This is a fork and modification of 
https://github.com/gregswift/ansible-ipaserver by Greg Swift. 


Requirements
------------

Primarily tested and functional on RHEL7, but open to others.

Role Variables
--------------

There are 2 main variables that need to be provided external to the role that have no default. 

    ipaserver_admin_password
    ipaserver_dir_admin_password

The following variables are used by this role and values are defined in 
    
    defaults/main.yml
    vars/[Operating System Name].yml

Base ipa-server install options
    
    ipaserver_base_command: ipa-server-install -U
    ipaserver_configure_ssh: True
    ipaserver_configure_sshd: True
    ipaserver_dns_forwarder: "{{ ansible_dns.nameservers[0] }}"
    ipaserver_domain: example.com
    ipaserver_hbac_allow: True
    ipaserver_idstart: 5000
    ipaserver_idmax: False
    ipaserver_mkhomedir: True
    ipaserver_realm: EXAMPLE.COM
    ipaserver_setup_dns: True
    ipaserver_setup_ntp: True
    ipaserver_ssh_trust_dns: True
    ipaserver_ui_redirect: True
    ipaserver_manage_firewalld: True
    ipaserver_admin_password: password
    ipaserver_dir_admin_password: password
    ipa_ip_address: "{{ ansible_default_ipv4.address }}"
    skip_install_dns_lookup: False
    ipa_gen_script_only: False
    
    firewall_services:
      - http
      - https
      - ldap
      - ldaps
      - kerberos
      - kpasswd
      - dns
      - ntp
       
    firewall_ports:
      - 88/tcp
      - 88/udp
      - 464/udp
      - 53/udp
      - 123/udp
      - 464/tcp
      - 53/tcp

    ipa_force_install: False


Example Playbook
----------------

Here is an example playbook that can readily wrap this role and still be fairly flexible.  You typically don't need to be this flexible on password source.

    ---
      - hosts: ipa
        remote_user: root
        become_user: root
        become: no
        roles:
        - role: ansible-ipaserver
          ipaserver_admin_password: letmein123
          ipaserver_dir_admin_password: letmein123
          ipa_ip_address: 192.168.100.2
          ipaserver_realm: testlab
          ipaserver_domain: testlab
          skip_install_dns_lookup: True
          ipa_gen_script_only: False

License
-------

GPLv2

Author Information
------------------

https://github.com/my0373/ansible-freeipa

Forked from - 
https://github.com/gregswift/ansible-freeipa
