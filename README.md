ansible-selinux-role
=========

Allows you to configure SELinux.

Config options:
* policy
* state
* booleans
* ports
* fcontexts

Requirements
------------

Tested on RHEL 7 and CentOS 7 only. 

Ansible 2.0 or above 

Role Variables
--------------
**It is not necessary to use all these variable blocks, you can use only the config options you really need.**


The following variables are used to configure SELinux policy and state:

```
    selinux_config: (optional, default: /etc/selinux/config)
    selinux_policy: (optional, default: targeted)
    selinux_state:  (optional, only values: enforcing|permissive|disabled, default: enforcing)
```

---

The following variables are used to toggle SELinux booleans: 

```
    selinux_boolean: 
      name_of_selinux_boolean:
        state: (optional, only values: yes|no default: yes)
        persistent: (optional, only values: yes|no, default: yes)
```

---

The following variables are used to configure SELinux ports: 

```
    selinux_ports: 
      name_of_selinux_type:
        ports: (required, port or port range)
        protocol: (optional, only values: tcp|udp default: tcp)
        state: (optional, only values: present|absent, default: present)
```

---

The following variables are used to configure SELinux fcontexts: 

```
    selinux_fcontext: 
      name_of_selinux_fcontext: (your own choice for better documentation in your playbook)
        file_spec: (required, regular expression to describe affected files)
        setype: (required, existing selinux type to label the files of your file_spec with it)
        ftype: (optional, only values: a|b|c|d|f|l|p|s - see manpage semanage-fcontext for file type, default: a - all files)
        state: (optional, only values: present|absent, default: present)
```

Example Playbook
----------------

```
    - hosts: server
      become: yes
      become_user: root
      become_method: su
      roles:
        - { role: ansible-selinux-role }
      vars:
        selinux_policy: "targeted"
        selinux_state: "enforcing"
        selinux_boolean:
          antivirus_can_scan_system:
            state: yes
            persistent: yes
          httpd_can_sendmail:
            state: yes
            persistent: yes
        selinux_ports:
          ssh_port_t:
            ports: 2222
            protocol: tcp
            state: present
          http_port_t:
            ports: 9000-9004
            protocol: tcp
            state: present
        selinux_fcontext:
          vcloud_documentroot:
            file_spec: "/srv/www(.*)"
            setype: httpd_sys_rw_content_t
            ftype: a
            state: present
```

License
-------

MIT
