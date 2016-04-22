## 循环                                                                                                                            
### 1.for
```
for planet in "Mercury 36" "Venus 67" "Earth 93" "Mars 142" "Jupiter 483"
do
  set -- $planet # 解析变量"planet"并且设置位置参数.
  # "--" 将防止$planet为空, 或者是以一个破折号开头.
  # 可能需要保存原始的位置参数, 因为它们被覆盖了.
  # 一种方法就是使用数组.
  # original_params=("$@")
  echo "$1 $2,000,000 miles from the sun"
  #-------two tabs---把后边的0和2连接起来
done
```

`for file in [jx]*` 当前目录下以"j"或"x"开头的文件

`for file in *`     当前目录下

### 2.while
```
a=1; while [ "$a" -le $LIMIT ] # ((a = 1)); while (( a <= LIMIT ))
do
#...
done
```
### 3.until
### 4.测试与分支(case与select结构)

- case (in) / esac

  ```
echo; echo "Hit a key, then hit return."
read Keypress

case "$Keypress" in
  abc ) echo "abc";;
  [[:lower:]] ) echo "Lowercase letter";;
  [[:upper:]] ) echo "Uppercase letter";;
  [0-9] ) echo "Digit";;
  * ) echo "Punctuation, whitespace, or other";;
esac
  ```
- select

  ```
# select variable [in list]
# do
#   TT CLASS="REPLACEABLE" >command...
#   break
# done
PS3='Choose your favorite vegetable: ' # 设置提示符字串.
select vegetable in "beans" "carrots" "potatoes" "onions" "rutabagas"
do
  echo
  echo "Your favorite veggie is $vegetable."
  echo
  break # 如果这里没有 'break' 会发生什么?
done
# git@iSource-123-72:~/ylp/bash$ bash a.sh 
# 1) beans
# 2) carrots
# 3) potatoes
# 4) onions
# 5) rutabagas
# Choose your favorite vegetable: 2
# 
# Your favorite veggie is carrots.
# 
# 如果这里没有 'break' go on--------------
# Choose your favorite vegetable:
  ```
