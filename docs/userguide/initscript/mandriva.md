# Mandriva Init Script

```sh
#!/bin/bash
#
# Startup script for the deluged daemon
#
# chkconfig: 2345 84 09
#
# description: deluged is the Deluge bit torrent daemon, 
# it performs downloads and manages torrents. 
# Connect to the service through the configured port.
# Script to manage start and stopping the mandriva service
# processname: deluged

### BEGIN INIT INFO
# Provides: deluged
# Should-Start:   $network
# Should-Stop:    $network
# Required-Start: $local_fs
# Required-Stop:  $local_fs
# Default-Start:  2 3 4 5
# Default-Stop:   0 1 6
# Short-Description: shadok
# Description: shadok is the Deluge bit torrent daemon,
# it performs downloads and manages torrents. 
# Connect to the service through the configured port.
# Script to manage start and stopping the mandriva service
# processname: deluged
### END INIT INFO

# Source function library.
. /etc/rc.d/init.d/functions

# Check that networking is up.
. /etc/sysconfig/network
[ "${NETWORKING}" = "no" ] && exit 0

DAEMON='deluged'
ACCOUNT='deluge'
HOME='/var/share/deluge'
# ----------- naming convention -----------

LOG="/var/log/$DAEMON"
PIDF="/var/run/${DAEMON}.pid"
LOCK="/var/lock/subsys/$DAEMON"
# ----------- basic checks -----------

[ ! -d "${HOME}" ] &&  gprintf "Can't find home %s, exit.\n" "${HOME}" && exit 1
[ -x '/usr/bin/deluged' -a -x '/usr/bin/deluge' ] || exit 0
#

case "$1" in
  daemon_start ) # real start
    [ `id -un` != "$ACCOUNT" ] && [ "$2" != '--test' ] && exit 3

    /usr/bin/deluged -c $HOME/.deluge/  -l $LOG -P $PIDF
    RETVAL=$?
    if [ $RETVAL -eq 0 ] ; then
       /usr/bin/deluge -u web -c $HOME/.deluge/ > $HOME/weluge.log 2>&1  &
       RETVAL=$?
       echo $!  >>  $PIDF
    fi
    ;;
  start)
    gprintf "Starting $DAEMON daemon: "
    touch  $LOG $PIDF
    chown $ACCOUNT:`id -g $ACCOUNT` $LOG $PIDF
    daemon -9 --user=$ACCOUNT --check $DAEMON /bin/ionice -c 3 $0 daemon_start
    RETVAL=$?
    echo
    [ $RETVAL -eq 0 ] && touch $LOCK
    ;;
  stop | smooth-stop)
    gprintf "Shutting down $DAEMON daemon: "
    [ -r "$PIDF" ] && pid=`cat $PIDF` 2>/dev/null
    #
    # kill first, think later
    #
    kill -TERM $pid >/dev/null 2>&1
    #
    # Saving state takes time: 10 sec
    #
    timeout=10
    for p in $pid ; do
       kill -s 0 $p 2>/dev/null
       for ((  ; 0==$?  && 0<$timeout ; timeout=$timeout - 1 )) do
          echo -n '.'
          sleep 1
          kill -s 0 $p 2>/dev/null
       done
    done
    if [ "$timeout" -eq 0 ] ; then
        failure "%s shutdown" $DAEMON
        kill -KILL $pid >/dev/null 2>&1
        RETVAL=1
    else
        success "%s shutdown" $base
        rm -f $LOCK
	RETVAL=0
    fi
    echo
    ;;
  hard-stop)
    gprintf "Fast stopping $DAEMON daemon: "
    #
    # no time to save state
    #
    killproc -d 1 $DAEMON
    RETVAL=$?
    echo
    [ $RETVAL -eq 0 ] && rm -f $LOCK
    ;;
  status)
    status $DAEMON
    RETVAL=$?
    ;;
  restart|reload)
    $0 stop
    $0 start
    ;;
  condrestart)
    [ -f $LOCK ] && $0 restart || status $DAEMON
    ;;
  *)
    gprintf "Usage: %s {start|stop|smooth-stop|hard-stop|status|restart}\n" "$0"
    RETVAL=1
    ;;
esac

exit $RETVAL

```