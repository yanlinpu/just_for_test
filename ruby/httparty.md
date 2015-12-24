[httparty-github](https://github.com/jnunemaker/httparty)

```
require 'httparty'
require 'json'
def post(uri,body)
  HTTParty.post(uri,
               :body=>body.to_json,
               :headers=>{ 'Content-Type' => 'application/json' })
end
params = {
  "params"=>"...",
  "arrar_params"=>[{"title"=>"no1"},{"title"=>"no2"}...]
}
response = post("#{host}api/v3/issues",params)
puts response.body, response.code, response.message, response.headers.inspect
```
## 简介
```
SupportedHTTPMethods =
[
  Net::HTTP::Get,
  Net::HTTP::Post,
  Net::HTTP::Patch,
  Net::HTTP::Put,
  Net::HTTP::Delete,
  Net::HTTP::Head,
  Net::HTTP::Options,
  Net::HTTP::Move,
  Net::HTTP::Copy
]
```
