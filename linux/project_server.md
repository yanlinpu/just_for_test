## 一个关于project的service

根据https://github.com/yanlinpu/information/blob/master/linux/service.md
```
#!/bin/bash                                                                                     

if [ $(id -u) -ne 0 ]; then
  echo "This script must be run as root"
  exit 1
fi

command="start|stop|restart|reload|status"
total=3
# IFS='|' read -r -a COMMAND <<< "$command"
# IFS='|' read -ra COMMAND <<< "$command"
# COMMAND=(${command//|/ }) #${command//|/ } 最后必须有空格 开始不能有空格
IFS='|' COMMAND=( $command )

APP_ROOT="/home/git/project_name"
DAEMON_OPTS="-c $APP_ROOT/config/unicorn.rb -E production"
PID_PATH="$APP_ROOT/tmp/pids"
UNICORN_PID="$PID_PATH/unicorn.pid"
SIDEKIQ_PID="$PID_PATH/sidekiq.pid"
KEEPGOD_PID="$PID_PATH/keepgod.pid"
KEEPGOD_LOG="/home/git/project_name/log/god.log"
KEEPGOD_CONF="$APP_ROOT/config/keep.god"
SIDEKIQ_STOP_CMD="RAILS_ENV=production bundle exec rake sidekiq:stop"
SIDEKIQ_START_CMD="RAILS_ENV=production bundle exec rake sidekiq:start"
DESC="project_name service"

DEFAULT_CONF="/etc/default/project_name"
UNICORN_ENABLED=1
SIDEKIQ_ENABLED=0       # Sidekiq is managed by GOD, so disable it.
KEEPGOD_ENABLED=1

# if file is exist, load it(settings)
if test -f $DEFAULT_CONF; then
  . $DEFAULT_CONF
fi

# the function of pid 
check_pidfile() {
  pidfile=$1
  if [ -n "$pidfile" ] && [ -e "$pidfile" ]; then
    pid=$(cat "$pidfile")
    # pid exist if file and ps 
    # ps -p <程序识别码> 　指定程序识别码，并列出该程序的状况
    # http://stackoverflow.com/questions/3043978/how-to-check-if-a-process-id-pid-exists
    if [ -n "$pid" ] && ps --pid $pid >/dev/null 2>&1; then
      return 0
    fi
  fi
  return 1
}

kill_pidfile() {
  if [ $# -eq 2 ]; then
    killopt=$1
    shift
  else
    killopt=-QUIT
  fi
  pidfile=$1
  if [ -n "$pidfile" ] && [ -e "$pidfile" ]; then
    pid=$(cat "$pidfile")
    if [ -n "$pid" ] && ps --pid $pid >/dev/null 2>&1; then
      kill $killopt $pid
      return $?
    fi
  fi
  return 1
}

# sidekiq start and stop
sidekiq_start() {
  if [ "$SIDEKIQ_ENABLED" = "0" ]; then
    echo >&2 "$DESC / Sidekiq is disabled"
    return 0
  fi
  if check_pidfile $SIDEKIQ_PID; then
    echo >&2 "WARN: $DESC / Sidekiq is currently running!"
  else
    printf "$DESC / Sidekiq startting...\t"
    sudo -u git -H bash -l -c "$SIDEKIQ_START_CMD > /dev/null"
    echo "done"
  fi
}

sidekiq_stop() {
  if check_pidfile $SIDEKIQ_PID; then
    printf "$DESC / Sidekiq stopping...\t"
    sudo -u git -H bash -l -c "$SIDEKIQ_STOP_CMD  > /dev/null"
    echo "done"
  fi
}

# unicorn start and stop
unicorn_start() {
  if [ "$UNICORN_ENABLED" = "0" ]; then
    echo >&2 "$DESC / Unicorn is disabled"
    return 0
  fi
  if check_pidfile $UNICORN_PID; then
    echo >&2 "WARN: $DESC / Unicorn is currently running!"
  else
    printf "$DESC / Unicorn startting...\t"
    sudo -u git -H bash -l -c "nohup bundle exec unicorn_rails $DAEMON_OPTS  > /dev/null  2>&1 &"
    echo "done"
  fi
}

unicorn_stop() {
  # Send signal QUIT to kill.
  kill_pidfile $UNICORN_PID
  rm -f "$UNICORN_PID" >/dev/null
  echo "$DESC / Unicorn stopped"
}

# keepgod start and stop
keepgod_start() {
  if [ "$KEEPGOD_ENABLED" = "0" ]; then
    echo >&2 "$DESC / God is disabled"
    return 0
  fi
  if check_pidfile $KEEPGOD_PID; then
    echo >&2 "WARN: $DESC / GOD is currently running!"
  else
    printf "$DESC / GOD startting...\t"
    sudo -i god -c "$KEEPGOD_CONF" -P "$KEEPGOD_PID" -l "$KEEPGOD_LOG"
    echo "done"
  fi
}

keepgod_stop() {
  if check_pidfile $KEEPGOD_PID; then
    printf "$DESC / GOD stopping...\t"
    sudo -i god terminate
    echo "done"
  fi
}

# every argument method
start() {
  cd $APP_ROOT
  unicorn_start
  sidekiq_start
  keepgod_start
}

stop() {
  cd $APP_ROOT
  keepgod_stop
  unicorn_stop
  sidekiq_stop
}

restart() {
  cd $APP_ROOT
  stop
  start
}

status() {
  if check_pidfile $UNICORN_PID; then
    echo "$DESC / Unicorn with PID $(cat $UNICORN_PID) is running."
  else
    echo "$DESC / Unicorn is not running."
  fi
  if check_pidfile $SIDEKIQ_PID; then
    echo "$DESC / Sidekiq with PID $(cat $SIDEKIQ_PID) is running."
  else
    echo "$DESC / Sidekiq is not running."
  fi
  if check_pidfile $KEEPGOD_PID; then
    echo "$DESC / GOD with PID $(cat $KEEPGOD_PID) is running."
    sudo -i god status
  else
    echo "$DESC / GOD is not running."
  fi
}

choice_select() {
  echo "you have only $total time(s) to choice"
  total=$[total-1]
  PS3='Choose your command: ' #提示符字串
  select num in "${COMMAND[@]}"; do
    check_validity $num
    break
  done
}

check_validity() {
  if [[ $total -gt 0 ]]; then
    case "$1" in
      start )
        start
        ;;
      stop )
        stop
        ;;
      restart )
        restart
        ;;
      reload )
        echo -n "Reloading $DESC configuration: "
        kill_pidfile -HUP $UNICORN_PID
        kill_pidfile -HUP $SIDEKIQ_PID
        echo "done."
        ;;
      status )
        status
        ;;
      * )
        choice_select
        ;;
    esac
  else
    echo "Usage: sudo service code $command or choice the num without arguments."
    exit 1
  fi
}

[ -d $PID_PATH ] || mkdir -p $PID_PATH

check_validity "$1"

exit 0
```

### 参考

- [Linux进程KILL－－Quit,INT,HUP,QUIT,和TERM的解释](http://blog.csdn.net/xifeijian/article/details/19286591)
- [stackoverflow--->How to check if a process id (PID) exists](http://stackoverflow.com/questions/3043978/how-to-check-if-a-process-id-pid-exists)
