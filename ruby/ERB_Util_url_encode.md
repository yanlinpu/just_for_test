## ruby rails 解析 url & html

1. ruby

  ```
require "erb"
include ERB::Util

puts html_escape("is a > 0 & a < 10?")  
puts h("is a > 0 & a < 10?")
# is a &gt; 0 &amp; a &lt; 10?

puts url_encode("a/bc?a#b \a")
puts u("a/bc?a#b \a")
# a%2Fbc%3Fa%23b%20%07
  ```

2. rails

  ```
# rails c
ERB::Util.url_encode("a/bc?a#b \a")
ERB::Util.u("a/bc?a#b \a")
# a%2Fbc%3Fa%23b%20%07
ERB::Util.html_escape("is a > 0 & a < 10?")
ERB::Util.h("is a > 0 & a < 10?")
is a &gt; 0 &amp; a &lt; 10?
  ```

###  [ruby ERB::Util api](http://ruby-doc.org/stdlib-2.1.2/libdoc/erb/rdoc/ERB/Util.html)
