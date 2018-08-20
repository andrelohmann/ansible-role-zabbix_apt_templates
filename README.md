zabbix_apt_templates
====================

Add apt update and needrestart templates to zabbix server and zabbix agents.

Requirements
------------

This role requires ubuntu xenials or bionic.

Role Variables
--------------

The following need to be set in group_vars/host_vars

    zabbix_url: __ZABBIX_SERVER_URL__
    zabbix_api_user: __ZABBIX_SERVER_ADMIN__
    zabbix_api_pass: __ZABBIX_SERVER_ADMIN_PASSWORD__
    zabbix_apt_templates_is_server: False
    zabbix_apt_templates_is_agent: False

Example Playbook
----------------

    - hosts: zabbix-server
      gather_facts: yes
      become: yes
      vars:
        zabbix_apt_templates_is_server: True
      roles:
      - andrelohmann.zabbix_server
      - andrelohmann.zabbix_apt_templates

    - hosts: zabbix-agent
      gather_facts: yes
      become: yes
      vars:
        zabbix_apt_templates_is_agent: True
        zabbix_host_groups:
        - Linux Servers
        zabbix_link_templates:
        - Template OS Linux
        - Template App APT Updates
        - Template App Needrestart
      roles:
      - andrelohmann.zabbix_agent
      - andrelohmann.zabbix_apt_templates

Dump JSON
---------

Templates are often exported as XML files, but the ansible module zabbix_template needs a JSON format. Exporting this JSON Format can be done by importing the XML file manually and then dump the Template as JSON using the template module.

```
# Export template json definition
- name: Dump Zabbix template
  local_action:
    module: zabbix_template
    server_url: "{{ zabbix_url }}"
    login_user: "{{ zabbix_api_user }}"
    login_password: "{{ zabbix_api_pass }}"
    template_name: Template App Needrestart
    state: dump
  register: _template_dump

- name: show template dump
  debug:
    msg: "{{ _template_dump }}"
```

Templates implemented
---------------------

https://share.zabbix.com/cat-app/app-other/apt-updates-monitoring
https://share.zabbix.com/cat-app/app-other/needrestart

License
-------

MIT

Author Information
------------------

https://github.com/andrelohmann
