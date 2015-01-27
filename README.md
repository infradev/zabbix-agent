infradev.zabbix-agent
=========

This is a role for installing and maintaining the Zabbix Pre-Compiled Agent, based on the dj-wasabi.zabbix-agent role.

Requirements
------------

This role will work on:
 * Red Hat
 * Debian
 * Ubuntu

Role Variables
--------------

There are some variables in de default/main.yml which can (Or needs to) be changed/overriden:
* `agent_server`: The ipaddress for the zabbix-server or zabbix-proxy.
* `agent_serveractive`: The ipaddress for the zabbix-server or zabbix-proxy for active checks.
* `zabbix_version`: This is the version of zabbix. Default it is 2.4, but can be overriden to 2.2 or 2.0.

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      sudo: yes
      roles:
         - { role: infradev.zabbix-agent, agent_server: 192.168.10.101, agent_serveractive: 192.168.10.101}

Extra Information
----------------

You can install so-called userparameter files by adding the following into your roles:
```
- name: Installing sample file
  copy: src=sample.conf
        dest="{{ agent_include }}/mysql.conf"
        owner=zabbix
        group=zabbix
        mode=0755
  notify: "{{ zabbix_agent }} restarted"
```
Example of the "sample.conf" file:
```
UserParameter=mysql.ping_to,mysqladmin -uroot ping | grep -c alive
```

You can extend your zabbix configuration by adding items yourself that do specific checks which aren't in the zabbix core itself. You can change offcourse the name of the file to whatever you want (Same for the UserParameter line(s))


Some SO installations may not have python-simplejson installed, witch is a dependency to Ansible on clients. To fix it execute the following command.

RedHat, Fedora, CentOS:
```
ansible hostname -m raw -a "yum -y install python-simplejson"
```

Debian, Ubuntu:
```
ansible hostname -m raw -a "apt-get -y install python-simplejson"
```

License
-------

BSD

Author Information
------------------

Github: https://github.com/infradev/zabbix-agent
