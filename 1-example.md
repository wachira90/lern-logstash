# Example1

## example file
````
nano /home/student/03-grok-examples/sample.log
````

## example content
````
2020-10-11T09:49:35Z INFO variable server value is tomcat
2020-03-14T22:50:34Z ERROR cannot find the requested resource
2020-01-02T14:58:40Z INFO initializing the bootup
2020-06-04T06:56:04Z DEBUG initializing checksum
2020-05-07T03:07:11Z INFO variable server value is tomcat
````

## config
````
nano /etc/logstash/conf.d/grok-example.conf
````

## content config
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
    match => { "message" => ['%{TIMESTAMP_ISO8601:time} %{LOGLEVEL:logLevel} %{GREEDYDATA:logMessage}'] }
  }
}
output {
  elasticsearch {
   hosts => "http://localhost:9200"
   index => "demo-grok"
}

stdout {}

}
````



## run with config
````
sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/grok-example.conf
````



## Let’s explore the contents added to the index:
````
curl -XGET "http://localhost:9200/demo-grok/_search?pretty=true" -H 'Content-Type: application/json' -d'{
  "_source": [
   "logLevel",
   "time",
   "logMessage"
  ]
}'
````



## output
````
 {
 "_index" : "demo-grok",
 "_type" : "_doc",
 "_id" : "FDyf9XEBIGK-cCtPEo5n",
 "_score" : 1.0,
 "_source" : {
 "logLevel" : "INFO",
 "logMessage" : "variable server value is tomcat",
 "time" : "2020-05-07T03:07:11Z"
 }
}
````
