# Automated Linux Monitoring & Alerting Script

## Overview

This project is a Bash-based Linux monitoring script that checks system health metrics such as:

- CPU usage
- Memory usage
- Disk usage
- Running processes
- System uptime

The script stores system health data into log files and generates alerts when disk usage exceeds a defined threshold.


## Features

- Logs system health information
- Monitors:
  - CPU usage
  - Memory usage
  - Disk usage
  - Running processes
- Generates alerts if threshold level exceeds 80%
- Stores logs in separate files


## Technologies Used

- Bash Scripting
- Linux Commands
- AWK
- SED

## Script

#!/bin/bash

LOGFILE="system_health.log"
ALERTFILE="alerts.log"

echo "=========================" >> $LOGFILE
date >> $LOGFILE
echo "=========================" >> $LOGFILE

# Memory
echo "Memory Usage" >> $LOGFILE
free -h >> $LOGFILE

# Disk
echo "Disk Usage" >> $LOGFILE
df -h >> $LOGFILE

# CPU
echo "CPU Usage" >> $LOGFILE
top -b -n 1 | head -10 >> $LOGFILE

# Disk threshold check
usage=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ "$usage" -gt 80 ]
then
    echo "$(date) ALERT: Disk usage greater than 80%" >> $ALERTFILE
fi 

## Run file

bash monitor.sh

## Sample output

Fri May 15 ALERT: Disk usage greater than 80%

## Log Files
system_health.log

Stores:

CPU details
Memory usage
Disk usage
System uptime
alerts.log

Stores alert messages when:

Disk usage exceeds 80%

## Automate using cron tab

Add cron job to run monitor.sh every 5 minutes 
crontab -e
*/5 * * * * /home/gani/monitor.sh

If threshold exceeds 80%, then alerts.log file contains the alerts in it.
