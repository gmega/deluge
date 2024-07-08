# openSUSE Init Script

## Deluge daemon (deluged)

```
#! /bin/sh
# Copyright (c) 1995-2000 SuSE GmbH Nuernberg, Germany.
#
# Author: José Ferrandis
#
# /etc/init.d/deluged
#
#   and symbolic its link
#
# /usr/sbin/deluged
#
# FROM http://dev.deluge-torrent.org/wiki/UserGuide/InitScript/OpenSUSE
#
### BEGIN INIT INFO
# Provides: deluged
# Required-Start: $local_fs $remote_fs
# Required-Stop: $local_fs $remote_fs
# Should-Start: $network
# Should-Stop: $network

# Default-Start: 3 5
# Default-Stop: 0 1 2 6
# Short-Description: Daemonized version of deluge.
# Description:       Starts the deluge daemon.
### END INIT INFO

DELUGED_BIN=/usr/bin/deluged

test -x $DELUGED_BIN || exit 5

DELUGED_USER="deluge"
test -n "$DELUGED_USER" || exit 6

DELUGED_PIDFILE=/home/$DELUGED_USER/.config/deluge/deluged.pid

DELUGED_ARGS="-c /home/$DELUGED_USER/.config/deluge/ -P $DELUGED_PIDFILE" # consult man deluged for more options

PYTHON_EGG_CACHE=/home/$DELUGED_USER/.python-eggs

. /etc/rc.status

# Shell functions sourced from /etc/rc.status:
#      rc_check         check and set local and overall rc status
#      rc_status        check and set local and overall rc status
#      rc_status -v     ditto but be verbose in local rc status
#      rc_status -v -r  ditto and clear the local rc status
#      rc_failed        set local and overall rc status to failed
#      rc_reset         clear local rc status (overall remains)
#      rc_exit          exit appropriate to overall rc status

# First reset status of this service
rc_reset

case "$1" in
    start)
        
        echo -n "Starting DELUGE daemon"
        ## Start daemon with startproc(8). If this fails
        ## the echo return value is set appropriate.
        
        export PYTHON_EGG_CACHE
        startproc -l /var/log/deluged.log -p $DELUGED_PIDFILE -u $DELUGED_USER $DELUGED_BIN $DELUGED_ARGS

        # Remember status and be verbose
        rc_status -v
        ;;
    stop)
        echo -n "Shutting down DELUGE daemon"
        ## Stop daemon with killproc(8) and if this fails
        ## set echo the echo return value.

        killproc -TERM $DELUGED_BIN

        # Remember status and be verbose
        rc_status -v
        ;;
    try-restart)
        ## Stop the service and if this succeeds (i.e. the 
        ## service was running before), start it again.
        $0 status >/dev/null &&  $0 restart

        # Remember status and be quiet
        rc_status
        ;;
    restart)
        ## Stop the service and regardless of whether it was
        ## running or not, start it again.
        $0 stop
        $0 start

        # Remember status and be quiet
        rc_status
        ;;
    force-reload|reload)
        ## Signal the daemon to reload its config. Most daemons
        ## do this on signal 1 (SIGHUP).

        echo -n "Reload service DELUGED"

        killproc -HUP $DELUGED_BIN

        rc_status -v

        ;;
    status)
        echo -n "Checking for service DELUGED "
        ## Check status with checkproc(8), if process is running
        ## checkproc will return with exit status 0.

        # Status has a slightly different for the status command:
        # 0 - service running
        # 1 - service dead, but /var/run/  pid  file exists
        # 2 - service dead, but /var/lock/ lock file exists
        # 3 - service not running

        checkproc $DELUGED_BIN

        rc_status -v
        ;;
    probe)
        ## Optional: Probe for the necessity of a reload,
        ## give out the argument which is required for a reload.

        ;;
    *)
        echo "Usage: $0 {start|stop|status|try-restart|restart|force-reload|reload|probe}"
        exit 1
        ;;
esac
rc_exit
```

## Web UI (deluge-web)

```
#! /bin/sh
# Copyright (c) 1995-2000 SuSE GmbH Nuernberg, Germany.
#
# Author: José Ferrandis
#
# /etc/init.d/deluge-webd
#
#   and symbolic its link
#
# /usr/sbin/deluge-webd
#
# FROM http://dev.deluge-torrent.org/wiki/UserGuide/InitScript/OpenSUSE
#
### BEGIN INIT INFO
# Provides: deluge-webd
# Required-Start: $local_fs $remote_fs
# Required-Stop: $local_fs $remote_fs
# Should-Start: $network deluged
# Should-Stop: $network

# Default-Start: 3 5
# Default-Stop: 0 1 2 6
# Short-Description: Daemonized version of deluge-web.
# Description:       Starts the deluge-web daemon.

### END INIT INFO

DELUGED_WEB_BIN=/usr/bin/deluge-web
test -x $DELUGED_WEB_BIN || exit 5

DELUGED_WEB_USER="deluge"

DELUGED_WEB_ARGS="-c /home/$DELUGED_WEB_USER/.config/deluge/ -f" # consult man deluge-web for more options

PYTHON_EGG_CACHE=/home/$DELUGED_WEB_USER/.python-eggs

. /etc/rc.status

# Shell functions sourced from /etc/rc.status:
#      rc_check         check and set local and overall rc status
#      rc_status        check and set local and overall rc status
#      rc_status -v     ditto but be verbose in local rc status
#      rc_status -v -r  ditto and clear the local rc status
#      rc_failed        set local and overall rc status to failed
#      rc_reset         clear local rc status (overall remains)
#      rc_exit          exit appropriate to overall rc status

# First reset status of this service
rc_reset

case "$1" in
    start)
        
        echo -n "Starting DELUGE-WEB daemon"
        ## Start daemon with startproc(8). If this fails
        ## the echo return value is set appropriate.

        export PYTHON_EGG_CACHE
        startproc -f -l /var/log/deluge-web.log -u $DELUGED_WEB_USER $DELUGED_WEB_BIN $DELUGED_WEB_ARGS

        # Remember status and be verbose
        rc_status -v
        ;;
    stop)
        echo -n "Shutting down DELUGE-WEB daemon"
        ## Stop daemon with killproc(8) and if this fails
        ## set echo the echo return value.

        killproc -TERM $DELUGED_WEB_BIN

        # Remember status and be verbose
        rc_status -v
        ;;
    try-restart)
        ## Stop the service and if this succeeds (i.e. the 
        ## service was running before), start it again.
        $0 status >/dev/null &&  $0 restart

        # Remember status and be quiet
        rc_status
        ;;
    restart)
        ## Stop the service and regardless of whether it was
        ## running or not, start it again.
        $0 stop
        $0 start

        # Remember status and be quiet
        rc_status
        ;;
    force-reload|reload)
        ## Signal the daemon to reload its config. Most daemons
        ## do this on signal 1 (SIGHUP).

        echo -n "Reload service DELUGED_WEB"

        killproc -HUP $DELUGED_WEB_BIN

        rc_status -v

        ;;
    status)
        echo -n "Checking for service DELUGED_WEB "
        ## Check status with checkproc(8), if process is running
        ## checkproc will return with exit status 0.

        # Status has a slightly different for the status command:
        # 0 - service running
        # 1 - service dead, but /var/run/  pid  file exists
        # 2 - service dead, but /var/lock/ lock file exists
        # 3 - service not running

        checkproc $DELUGED_WEB_BIN

        rc_status -v
        ;;
    probe)
        ## Optional: Probe for the necessity of a reload,
        ## give out the argument which is required for a reload.

        ;;
    *)
        echo "Usage: $0 {start|stop|status|try-restart|restart|force-reload|reload|probe}"
        exit 1
        ;;
esac
rc_exit
```