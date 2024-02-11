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

## Gather DCS Info for an SR

This is a script to quickly gather some useful DCS Info to include in Oracle Support Reqeusts.

```
# Run this to find a failed job to investigate
sudo -s
/opt/oracle/dcs/bin/dbcli list-jobs | grep Failure

# Save the Job ID as a variable
JOB_ID=0920dd87-3b70-4c58-8e9a-d87d85231950 # ID of Failed Job
```
```
# Run this script to collect DCS Info in a temp directory and zip file
DCS_HOME=/opt/oracle/dcs
INFODIR=$(mktemp -d -t dcsinfo.XXXX)

# DCS tools info
echo "DCS component versions"   >> ${INFODIR?}/dcstools.info
echo "========================" >> ${INFODIR?}/dcstools.info
sudo rpm -qa|egrep 'dcs'        >> ${INFODIR?}/dcstools.info
echo -e >> ${INFODIR?}/dcstools.info

echo "DCS services status"      >> ${INFODIR?}/dcstools.info
echo "========================" >> ${INFODIR?}/dcstools.info
systemctl status initdcsadmin   >> ${INFODIR?}/dcstools.info
systemctl status initdcsagent   >> ${INFODIR?}/dcstools.info

cp ${DCS_HOME?}/log/dcs-admin.* ${INFODIR?}/
cp ${DCS_HOME?}/log/dcs-agent.* ${INFODIR?}/
cp ${DCS_HOME?}/log/dcs-agent-debug.* ${INFODIR?}/

# dbcli jobs info
${DCS_HOME?}/bin/dbcli list-jobs   >> ${INFODIR?}/dbcli-list-jobs.all
${DCS_HOME?}/bin/dbcli list-jobs | grep Failure >> ${INFODIR?}/dbcli-list-jobs.failed

# dbcli describe job
cp ${DCS_HOME?}/log/jobs/${JOB_ID?}.log ${INFODIR?}/
${DCS_HOME?}/bin/dbcli describe-job -l Verbose -i ${JOB_ID?} >> ${INFODIR?}/dbcli-describe-job.${JOB_ID?}
RMAN_LOG=$(${DCS_HOME?}/bin/dbcli describe-job -l Verbose -i ${JOB_ID?} | grep "Log File" | cut -c12-)
RMAN_LOG=${RMAN_LOG::-2}
cp ${RMAN_LOG?} ${INFODIR?}/

# zip up DCS Info
zip -r ${INFODIR?}/dcsinfo.zip ${INFODIR?}

# Complete
echo "DCS Info was collected in ${INFODIR}"
echo -e 
ls -la "${INFODIR}"

```

## Reference

[Oracle Database CLI Reference](https://docs.oracle.com/en-us/iaas/dbcs/doc/oracle-database-cli-reference.html){:target="_blank"}
