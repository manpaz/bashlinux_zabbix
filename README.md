bashlinux_zabbix
================

Ansible role to install zabbix agent and server.

Requirements
------------

Zabbix server requires the following resources to be installed and configured correctly
- A database: MySQL, MariaDB or PostgreSQL
- A webserver: Apache or NGinx
- PHP-FPM

Eventually these requirements will be de-coupled to their own role.

Role Variables
--------------

The settable variables for this role are:
```yaml
zabbix_server_ip:       # IP address of the Zabbix server.
                        # (mandatory)
zabbix_server:          # Flag enabling the installation and configuration of Zabbix server, flag can be set either on a specified host group or host name.
                        # (default false)
zabbix_agent:           # Flag enabling the installation and configuration of Zabbix agent, flag can be set either on a specified host group or host name.
                        # (default true)
mysql_datadir:          # Location of mysql data dir
                        # (default /mnt/dbs/mysql)
mysql_database:         # Database name for the Zabbix server installation
                        # (default zabbix)
mysql_user:             # Database user for the Zabbix server installation
                        # (default zabbix)
mysql_pass:             # Database password for the mysql user created for the Zabbix server installation
                        # (mandatory)
php_timezone:           # PHP time zone for the Zabbix server front-end installation
                        # (default empty)
web_servername:         # Server name for the Zabbix server front-end installation
                        # (default empty)
```

Dependencies
------------

- firewalld: Allow traffic on HTTP(S) TCP ports 80 and 443 and Zabbix agent and trapper TCP ports 10050 an 10051 respectively
- selinux: Allow zabbix to create a socket file, communicate with the web server and the database.

Example Playbook
----------------

Install zabbix agent in all servers and set the IP of `bastion-server` on the agent configuration.

```yaml
---
- hosts: servers
  vars:
    zabbix_server_ip: "{{ hostvars['bastion-server']['ansible_facts']['default_ipv4']['address'] }}"
  roles:
    - bashlinux_zabbix
```

To install Zabbix server on `bastion-server` just enable the flag `zabbix_server` on its variable file
```yaml
---
# file: host_vars/bastion-server
zabbix_server: true
```

License
-------

LGPL-3.0-or-later
