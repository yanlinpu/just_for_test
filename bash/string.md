## 操作字符串

### 基本操作

- 字符串长度

  ```
a="abcd 4"
echo ${#a}
echo "${#a}" 
echo `expr length "$a"` # 必须带 ""
echo `expr "$a" : '.*'`

# ${#var}字符串长度(变量$var得字符个数). 对于array来说, ${#array}表示的是数组中第一个元素的长度.
# ${#*}和${#@}表示位置参数的个数.
# 对于数组来说, ${#array[*]}和${#array[@]}表示数组中元素的个数.
  ```
- 匹配字符串开头的子串长度

  ```
# expr match "$string" '$substring' 
  # $substring是一个正则表达式.
a="abcABC123ABCabc"
echo `expr match "$a" 'abc[A-Z]*.2'`
echo `expr "$a" : 'abc[A-Z]*.2'`
  ``` 
- 索引

  ```
#expr index $string $substring
a="abcABC123ABC1234abc"
echo `expr index "$a" "C1234"` 
# 6 ====> C (in #6 position) matches before others.
echo `expr index "$a" Da` # 1
  ```
- 提取子串

  ```
# 1. ${string:position}
# 如果$string是"*"或者"@", 那么将会提取从位置$position开始的位置参数
# ${string:position:length}
a="abcABC123ABCabc"
echo ${a:7:3}  # 23A
# 从字符串的右边(也就是结尾)部分开始提取子串
echo ${a:(-4)} # Cabc
echo ${a: -4 } # Cabc
# 使用圆括号或者添加一个空格可以"转义"这个位置参数
echo ${a:-4 }  # abcABC123ABCabc
# 默认是提取整个字符串, 就象${parameter:-default}一样.

# 如果$string是"*"或者"@", 那么将会提取从位置$position开始的位置参数
echo ${*:2}   # bash a.sh  aaa bbb ccc ddd eee fff => bbb ccc ddd eee fff
echo ${*:2:3} # bash a.sh  aaa bbb ccc ddd eee fff => bbb ccc ddd

# 2.expr substr $string $position $length
echo `expr substr "$a" 1 2 # ab

# 3.expr match "$string" '\($substring\)'
# expr "$string" : '\($substring\)'
echo `expr match "$a" '\(.[b-c]*[A-Z]..[0-9]\)'` # abcABC1
echo `expr "$a" : '\(.[b-c]*[A-Z]..[0-9]\)'`

# 4.expr match "$string" '.*\($substring\)'
# expr "$string" : '.*\($substring\)'
# 从$string的结尾提取$substring, $substring是正则表达式
echo `expr "$a" : '.*\(......\)'` # ABCabc
  ```
- 子串削除

  ```
# 1.${string#substring}
  # 从$string的开头位置截掉最短匹配的$substring.
# ${string##substring}
  # 从$string的开头位置截掉最长匹配的$substring.
stringZ=abcABC123ABCabc
echo ${stringZ#a*C}  # 123ABCabc
echo ${stringZ##a*C} # abc

# 2.${string%substring}
  # 从$string的结尾位置截掉最短匹配的$substring.
# ${string%%substring}
  # 从$string的结尾位置截掉最长匹配的$substring.
  ```
- 子串替换

  ```
# 1.${string/substring/replacement}
  # 使用$replacement来替换第一个匹配的$substring.
# ${string//substring/replacement}
  # 使用$replacement来替换所有匹配的$substring.
stringZ=abcABC123ABCabc 
echo ${stringZ/abc/xyz}  # xyzABC123ABCabc
echo ${stringZ//abc/xyz} # xyzABC123ABCxyz

# 2.${string/#substring/replacement}
  # 如果$substring匹配$string的开头部分, 那么就用$replacement来替换$substring.
# ${string/%substring/replacement}
  # 如果$substring匹配$string的结尾部分, 那么就用$replacement来替换$substring.
echo ${stringZ/#abc/XYZ} # XYZABC123ABCabc
echo ${stringZ/%abc/XYZ} # abcABC123ABCXYZ
# stringZ=1abcABC123ABCabc2 则以上两条结果都为 => 1abcABC123ABCabc2 
  ```
- 使用awk来处理字符串

  ```
String=23skidoo1
echo ${String:2:4} #=> skid
echo | awk '{ print substr("'"${String}"'",3,4) }' #=> skid
  ```
  
### 参数替换
```
# 1.${parameter-default}, ${parameter:-default}
# ${parameter-default} -- 如果变量parameter没被声明, 那么就使用默认值.
# ${parameter:-default} -- 如果变量parameter没被设置, 那么就使用默认值.
variable=
echo ${abc-00}      # 00
echo ${abcd:-11}    # 11
echo ${variable-0}  # ''
echo ${variable:-1} # 1

# 2.${parameter=default}, ${parameter:=default}
# ${parameter=default} -- 如果变量parameter没声明, 那么就把它的值设为default.
# ${parameter:=default} -- 如果变量parameter没设置, 那么就把它的值设为default.

# 3.${parameter+alt_value}, ${parameter:+alt_value}
# ${parameter+alt_value} -- 如果变量parameter被声明了, 那么就使用alt_value, 否则就使用null字符串. 
# ${parameter:+alt_value} -- 如果变量parameter被设置了, 那么就使用alt_value, 否则就使用null字符串. 

# 4.${parameter?err_msg}, ${parameter:?err_msg}
# ${parameter?err_msg} -- 如果parameter已经被声明, 那么就使用设置的值, 否则打印err_msg错误消息. 
# ${parameter:?err_msg} -- 如果parameter已经被设置, 那么就使用设置的值, 否则打印err_msg错误消息. 
```

### 指定变量的类型: 使用declare或者typeset

#### declare/typeset选项
- -r 只读

  ``` 
declare -r var1
  ``` 
- -i 整型

  ``` 
declare -i number
number=three
echo "Number = $number" # Number = 0
  ```
- -a 数组
- -f 函数

  - `declare -f` 不加任何参数那,么将会列出这个脚本之前定义的所有函数
  - `declare -f function_name` 只会列出这个函数的名字
- -x export

  ```
# 这句将会声明一个变量, 并作为这个脚本的环境变量被导出.
  ```
- -x var=$value

### 间接引用
```
a=letter_of_alphabet # 变量"a"的值是另一个变量的名字.
letter_of_alphabet=z
# 直接引用.
echo "a = $a" # a = letter_of_alphabet
# 间接引用.
eval a=\$$a
echo "Now a = $a" # 现在 a = z
```

### $RANDOM: 产生随机整数
$RANDOM是Bash的内部函数 (并不是常量), 这个函数将返回一个伪随机整数,范围在0 - 32767之间.
```
Arr="1
2
3"
arr=($Arr)                         # 1
num_arr=${#arr[*]}                 # 3
echo "${arr[$((RANDOM%num_arr))]}" # 1 or 2 or 3
```
