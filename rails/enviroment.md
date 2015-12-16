## enviroment 调用配置文件的配置

```
#config/application.yml
redis
  host: localhost
  port: 6379
  db: 0
  namespace: test
#config/enviroments/production.rb  
...
  APP_CONFIG = YAML.load_file(Rails.root.join('config', 'application.yml'))
  config.cache_store = :redis_store, { :host => APP_CONFIG['redis']['host'],
      :port => APP_CONFIG['redis']['port'],
      :db   => APP_CONFIG['redis']['db'],
      #:password => "mysecret",
      #:expires_in => 90.minutes,
      :namespace => APP_CONFIG['redis']['namespace']
  }
...  
```
