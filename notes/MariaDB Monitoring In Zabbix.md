---
attachments: [Clipboard_2024-04-06-11-09-50.png]
tags: [Documentation]
title: MySQL/MariaDB Monitoring In Zabbix
created: '2024-04-06T14:58:37.942Z'
modified: '2024-04-06T16:50:53.457Z'
---

# MySQL/MariaDB Monitoring In Zabbix

## Purpose
This document will outline the process of configuring MySQL/MariaDB monitoring via Zabbix **on a host where basic monitoring via Zabbix Agent 2** has already established. 

## Prerequisites
- Zabbix Agent 2 installed and configured on the host
- Basic monitoring established to the host on Zabbix

## MySQL Host Configuration

### User Creation
> There are a couple of ways to do this depending on what you're trying to achieve. But for the purposes of this document, we'll be creating a user that's only meant to monitor database performance
>> As always, replace things in <> with their proper values

1. Create a MySQL user that will serve as our "monitor"
`CREATE USER 'zabbix_user'@'<server_IP>' IDENTIFIED BY '<password>';`

2. Grant this user the necessary permissions to monitor the MySQL server
`GRANT USAGE, REPLICATION, CLIENT, PROCESS, SHOW DATABASES, SHOW VIEW ON *.* TO 'zabbix_user'@'<server_IP>';`

### Allowing MySQL Remote Access 
> Depending on which flavour of MySQL is being used, the config file might be in a couple of different places. 
1. Navigate to MySQL's settings directory
`cd /etc/mysql/mysql.conf.d`

2. Open the config file and look for the line that begins with `bind-address`
![](@attachment/Clipboard_2024-04-06-11-09-50.png)

3. Replace that with the address from which we want to allow remote connections then save and close the config file. 

4. Restart MySQL to make sure the changes take effect


## Zabbix Host Configuration
> If you happen to be running Zabbix with a MySQL backend, you should test that you're able to remotely log into MySQL on the remote server. `mysql -u zabbix_user -h <server_IP -p>`

1. Go to the Monitoring → Hosts page, find the host and click to configure it 

2. Add the template `MySQL by Zabbix Agent 2` to the host

3. Click on the Macros tab at the top to view the macros for this host 

4. We'll need to create three Macros for our connection parameters 
    - `{$MYSQL.DSN}` - `tcp://<server_IP>:3306`
    - `{$MYSQL.USER}` - `zabbix_user`
    - `{$MYSQL.PASSWORD}` - `<password>` 
