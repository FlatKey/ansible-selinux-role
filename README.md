ansible-selinux-role
=========

Allows you to configure SELinux.

Config options:
* policy
* state
* booleans

Requirements
------------

Tested on RHEL 7 and CentOS 7 only. 

Ansible 2.0 or above 

Role Variables
--------------

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
```

License
-------

MIT
