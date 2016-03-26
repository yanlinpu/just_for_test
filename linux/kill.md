## Linux下批量Kill多个进程的方法
```
$  ps aux | grep "进程名" | awk '{print $2 }' | xargs kill -9
$  ps aux | grep "进程名" | cut -c 9-15 | xargs kill -9
```

#### [awk](http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html)
- 使用方法  
    `$ awk '{pattern + action}' {filenames}`  
    `$ awk [-F  field-separator]  'commands'  input-file(s)`  
    ommands 是真正awk命令，[-F域分隔符]是可选的。在awk中，文件的每一行中，由域分隔符分开的每一项称为一个域。通常，在不指名-F域分隔符的情况下，默认的域分隔符是空格。
- 使用demo    
    - `$ cat /etc/passwd |awk  -F ':'  'BEGIN {print "name,shell"}  {print $1","$7} END {print "blue,/bin/nosh"}'`  
    - `$ awk -F : '/root/{print $7}' /etc/passwd`   
    - `$ awk 'BEGIN {count=0;print "[start]user count is ", count} {count=count+1;print $0;} END{print "[end]user count is ", count}' /etc/passwd`    
    - `$ ls -l |awk 'BEGIN {size=0;print "[start]size is ", size} {if($5!=4096){size=size+$5;}} END{print "[end]size is ", size/1024/1024,"M"}' `    
    - `$ awk -F ':' 'BEGIN {count=0;} {name[count] = $1;count++;}; END{for (i = 0; i < NR; i++) print i, name[i]}' /etc/passwd`   

#### xargs

`$ mkdir logs; find . -name "*.log" | xargs -i mv {} logs`  
`$ mkdir logs; find . -name "*.log" | xargs -I [] mv [] logs`  
`$ find . -name "*.log" | xargs -p rm -f`  
`$ find . -name "*.log" | xargs -i -p rm -f {}`  
- 使用-i参数默认的前面输出用{}代替，-I参数可以指定其他代替字符
- -p参数会提示让你确认是否执行后面的命令,y执行，n不执行
- -p -i 配合使用每执行一条询问一次
