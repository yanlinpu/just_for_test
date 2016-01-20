#### no1.$~ holds a MatchData object. ::last_match is equivalent to $~.
```
/a/ =~ 'haystack' # ==> 1
$~                # ==> #<MatchData "a">
```
#### no2.命名 ?<name> | ?'name'
```
/\$(?<dollars>\d+)\.(?<cents>\d+)/.match("$3.67")
=====>#<MatchData "$3.67" dollars:"3" cents:"67">
/\$(?<dollars>\d+)\.(?<cents>\d+)/.match("$3.67")[:cents]
/\$(?<dollars>\d+)\.(?<cents>\d+)/.match("$3.67")[2]
$2
=====> 67
# =~ 可以把name改变为本地变量 
/\$(?<dollars>\d+)\.(?<cents>\d+)/ =~ "$3.67"
=====> 0
cents
$2
=====> 67
```
#### no3.命名引用 \k<name>
```
/(?<vowel>[aeiou]).\k<vowel>.\k<vowel>/.match('ototomy')
======>#<MatchData "ototo" vowel:"o">
```
#### no4. (?:) 只分组，不进行 \1 反向引用
```
/I(n)ves(ti)ga\2ons/.match("Investigations")
=====> #<MatchData "Investigations" 1:"n" 2:"ti">
/I(?:n)ves(ti)ga\1ons/.match("Investigations")
=====> #<MatchData "Investigations" 1:"ti">
```
#### no5. (?>pat) atomic grouping原子分组
```
/".*"/.match('"Quote"')       #=> #<MatchData "\"Quote\"">
/"(?>.*)"/.match('"Quote"')   #=> nil
/"(?>.{5})"/.match('"Quote"') #=> #<MatchData "\"Quote\"">
```
#### no6.子表达式调用 \g
```
/\A(?<paren>\(\g<paren>*\))*\z/ =~ '()'
/\A(?<paren>\(\g<paren>*\))*\z/ =~ '((()))'
======> 0
```

#### no7.  (?=pat)匹配到pat但不包括 (?!pat)匹配到不是pat不包括 (?<=pat)匹配pat后面 不包括 (?<!pat)匹配不是pat后面不包括
```
/(?<=<b>)\w+(?=<\/b>)/.match("Fortune favours the <b>bold</b>")  #bold
```

#### no8. [ruby-china/one_demo](https://ruby-china.org/topics/28655)
```
"$aa ,  bb=cc dd".match /(?<=^\$)(\p{Word}+)(?:\s*,\s*|\s+)(\p{Word}+)(?=\s*\=)/
 => #<MatchData "aa ,  bb" 1:"aa" 2:"bb">
 "$aa ,  bb=cc dd".gsub(/(?<=^\$)(\p{Word}+)(?:\s*,\s*|\s+)(\p{Word}+)(?=\s*\=)/, '\1_\2')
 => "$aa_bb=cc dd"
 # 分组先匹配到$符号但不引用，分组（1）求得第一个单词，分组空格和逗号但不引用，再分组（2）求得第二个单词，以空格或者等号结束匹配
```
## 正则资源

http://ruby-doc.org/core-2.1.2/Regexp.html#method-i-source

http://www.itupup.com/?it02/517475.htm

http://blog.donews.com/maverick/archive/2005/11/28/641232.aspx
