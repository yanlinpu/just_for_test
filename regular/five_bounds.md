## 正则表达式 边界符中的 ^, $, \A, \Z, \z

- ^ 和 $ 分别代表一行（line）的开始和结束的位置；
- \A 和 \z 分别代表输入（input）的开始和结束位置；
- \Z 代表输入的结尾位置，但是字符串的结尾可以有也可以没有终止子（final terminator：\n, \r, \r\n, \u0085, \u2028, \u2029）。
```
2.1.2 :030 > "A is right.\nB is right too." =~ /^B/
 => 12 
2.1.2 :031 > "A is right.\nB is right too." =~ /right.$/
 => 5 
2.1.2 :032 > "A is right.\nB is right too." =~ /right.\z/
 => nil 
2.1.2 :033 > "A is right.\nB is right too." =~ /\AB/
 => nil 
2.1.2 :034 > "A is right.\nB is right too.\n" =~ /too.\z/
 => nil 
2.1.2 :035 > "A is right.\nB is right too.\n" =~ /too.\Z/
 => 23 
```
