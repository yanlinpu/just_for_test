## /etc/init.d/a_server

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

# every argument method
start() {
  echo 'start'
}

stop() {
  echo 'stop'
}

restart() {
  echo 'restart'
}

reload() {
  echo 'reload'
}

status() {
  echo 'status'
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
        reload
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

check_validity "$1"

exit 0    
```

#### unrecognized service：
```
$ server a_server
# a_server: unrecognized service
$ sudo chmod +x /etc/init.d/a_service
```
