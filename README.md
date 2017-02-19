Role Name
=========

This ansible role is to install dbforbix zabbix proxy. It monitors databases such as mysql and oracle at the moment. The original project link: https://github.com/vagabondan/DBforBIX

Requirements
------------

Application requires jdk 1.8, git and systemd. dbforbix role was tested on centos 7.

Role Variables
--------------

You can see full list of default variables in defaults/main.yml .

dbforbix_git_repo - URL to dbforbix project. Default: https://github.com/vagabondan/dbforbix.git
dbforbix_path - path for git clone. Default: /opt/dbforbix
dbforbix_base_path - path from git clone to application base dir. Default: dist/db4bix
dbforbix_git_ref - commit to clone. Default: HEAD
dbforbix_jar_file - jar file to start. Default: dbforbix_run_2.3-beta.jar

dbforbix_log_level - log level. Default: All
dbforbix_log_file - log file. Default: /var/log/db4bix.log
dbforbix_log_file_size - log file size. Default: 10M
dbforbix_sleep - interval to monitor hosts. Default: 30
dbforbix_update_config_timout - interval to update configuration of service. Default: 120
dbforbix_max_thread_number - max thread number. Default: 100
dbforbix_pool_max_active - max active connections in a pool. Default: 10
dbforbix_pool_query_timout - query timeout. Default: 15

dbforbix_zabbix_servers - list of zabbix server. It's an array, every element there is a dictanory. Element variables:
  name - name of zabbix server
  address - address of zabbix server
  port - port of zabbix server. Default: 10051
  proxy_name - proxy name
  db_list - list of databases to monitor

dbforbix_oracle - list of Oracle databases to monitor. It's an array, every element there is a dictanory. Element variables: 
  name - name of Oracle DB. Must be the same as from db_list.
  instance - name of instance (database to monitor)
  address - address of Oracle server
  port - port of Oracle server. Default: 1521
  user - user for connection to Oracle
  password - password for connection to Oracle
  max_active - max active connections to server. Default: 10
  max_wait_millis - max time of waiting. Default: 10000
  max_idle - max idle. Default: 1

dbforbix_mysql - list of mysql databases to monitor. It's an array, every element there is a dictanory. Element variables: 
  name - name of mysql DB. Must be the same as from db_list.
  instance - name of databse to monitor
  address - address of mysql server
  port - port of mysql server. Default: 3306
  user - user for connection to mysql
  password - password for connection to mysql
  max_active - max active connections to server. Default: 10
  max_wait_millis - max time of waiting. Default: 10000
  max_idle - max idle. Default: 1

Dependencies
------------

You can use geerlingguy.java ansible galaxy role for java installation and geerlingguy.git for git.

Example Playbook
----------------

An example of playbook with dbforbix role and 2 different databases.

    - hosts: servers
      roles:
      - role: vladislavtomenko.dbforbix
        dbforbix_zabbix_servers:
        -
          name: Zabbix1
          address: 127.0.0.1
          port: 10051
          proxy_name: dbforbix
          db_list:
          - MYSQLDB1
          - ORADB1
        dbforbix_oracle:
        - 
          name: ORADB1
          instance: orcl12c
          address: 192.168.1.31
          port: 1521
          user: zabbix
          password: zabbix
        dbforbix_mysql:
        - 
          name: MYSQLDB1
          instance: testdb
          address: 192.168.1.31
          port: 3306
          user: zabbix
          password: zabbix

License
-------

Apache License
