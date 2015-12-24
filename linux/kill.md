## Linux下批量Kill多个进程的方法
```
$  ps aux | grep "进程名" | awk '{print $2 }' | xargs kill -9
$  ps aux | grep "进程名" | cut -c 9-15 | xargs kill -9
```
