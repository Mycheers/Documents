#!/bin/bash
#
LOGFILE="/tmp/daemon_node.log"
TIME=`date "+%Y%m%d %H%M%S"`
CHECK_NODE=`echo ""|telnet 192.168.71.246 1888 2>/dev/null|grep "\^]"|wc -l`

if [ $CHECK_NODE -ne 1 ];then
    cd /usr/local/otter/node/bin/ && ./stop.sh
    sleep 5
    echo "$TIME The node service is stopped." >> $LOGFILE
    cd /usr/local/otter/node/bin/ && ./startup.sh
    sleep 5
    echo "$TIME The node service is running." >> $LOGFILE
fi




