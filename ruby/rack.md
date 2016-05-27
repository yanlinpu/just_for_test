## 检查代理的可用性

### proxy.sh
```
#!/bin/sh                                                                                

PROXYS=(10.167.159.45:8889 10.57.89.177:8899 10.57.91.89:8889)
for i in "${PROXYS[@]}"
do
  curl -x http://$i --connect-timeout 1 www.baidu.com 1>/dev/null 2>&1
  if [ $? == 0 ]; then
    echo "$i"
  fi  
done
```

### 第一种web访问方法 proxy.rb
```
#调用 ruby proxy.ru 然后网页访问localhost:8080
require 'webrick'
server = WEBrick::HTTPServer.new Port: 8080

class Handler < WEBrick::HTTPServlet::AbstractServlet
  def do_GET req, res 

    if req.path == '/' 
      output = `bash proxy.sh`
      res.body = output.to_s
    else
      res.body = "hehe"
    end 
  end 
end

server.mount '/', Handler

trap 'INT' do server.shutdown end 

server.start
```

### 第二种web访问方法 proxy.rb
```
#调用 ruby proxy.rb 然后网页访问localhost:8080
require 'rack'                                                                           

class ProxyUseful
  def call(env)
    output = `bash proxy.sh`
    ['200', {'Content-Type' => 'text/html'}, [output.to_s]]
  end 
end

Rack::Handler::WEBrick.run ProxyUseful.new, :Port => 8080
```

### 第三种web访问方法 proxy.ru
```
#调用 rackup proxy.rb -p 8080 然后网页访问localhost:8080
require 'rack'                                                                           

class ProxyUseful
  def call(env)
    output = `bash proxy.sh`
    ['200', {'Content-Type' => 'text/html'}, [output.to_s]]
  end 
end

run ProxyUseful.new
```
