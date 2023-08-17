---
title: dbcli
layout: page
permalink: /posts/oracledb/dbcli/
parent: Oracle Database
#nav_order: 1
---

# Oracle Database CLI

The Database CLI (dbcli) is a command line interface available for Base Database Service. Below you will find some useful commands and other information about using `dbcli` to manage Oracle database homes and databases.

## Version

```
sudo rpm -qa|egrep 'dcs|exa|dbaas'
```

## Update

It is a good idea to keep the `dbcli` components up to date. Use the `cliadm update-dbcli` to update to the latest versions. If you are running RAC, make sure to run this on both nodes.

```
sudo rpm -qa|egrep 'dcs|exa|dbaas'
sudo /opt/oracle/dcs/bin/cliadm update-dbcli
sudo /opt/oracle/dcs/bin/cliadm list-jobs | tail
sudo rpm -qa|egrep 'dcs|exa|dbaas'
```

## Jobs

TODO

## Logs

TODO

## Reference

[Oracle Database CLI Reference](https://docs.oracle.com/en-us/iaas/dbcs/doc/oracle-database-cli-reference.html){:target="_blank"}
