# 3 Multiple Grok Filters to Parse Complex Files


## open config 
````
nano /etc/logstash/conf.d/grok-example-02.conf
````

## config
````
input {
  file {
   path => "/home/student/03-grok-examples/sample.log"
   start_position => "beginning"
   sincedb_path => "/dev/null"
  }
}

filter {
  grok {
   match => { "message" => [
   '%{TIMESTAMP_ISO8601:time} %{LOGLEVEL:logLevel} %{GREEDYDATA:logMessage}', '%{IP:clientIP} %{WORD:httpMethod} %{URIPATH:url}'
   ] }
 }
}

output {
  elasticsearch {
   hosts => "http://localhost:9200"
   index => "demo-grok-multiple"
 }
stdout {}
}
````

## Once again, we press CTRL+X, followed by Y and then ENTER to save the file. We notice the change in the config file is the new line added to the match option:

` '%{IP:clientIP} %{WORD:httpMethod} %{URIPATH:url}'  `

````
match => { "message" => [
   '%{TIMESTAMP_ISO8601:time} %{LOGLEVEL:logLevel} %{GREEDYDATA:logMessage}', '%{IP:clientIP} %{WORD:httpMethod} %{URIPATH:url}'
   ] }
````

## see what has been added to the index:
````
curl -XGET "http://localhost:9200/demo-grok-multiple/_search?pretty" -H 'Content-Type: application/json' -d'{
  "_source": {
   "excludes": [
    "@timestamp",
   "host",
  "path"
  ]
 }
}'
````
