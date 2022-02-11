# 2 example

How Grok Deals with Lines that Don’t Match Patterns

## sample log:
````
nano /home/student/03-grok-examples/sample.log
````

## content
````
2020-10-11T09:49:35Z INFO variable server value is tomcat
2020-03-14T22:50:34Z ERROR cannot found the requested resource
2020-01-02T14:58:40Z INFO initializing the bootup
2020-06-04T06:56:04Z DEBUG initializing checksum
2020-05-07T03:07:11Z INFO variable server value is tomcat
55.12.32.134 GET /user/id/properties
````

## if delete index
````
curl -XDELETE "http://localhost:9200/demo-grok"
````

## run with config
````
sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/grok-example.conf
````

## After the job is done, we press CTRL+C to exit. Let’s see what our index looks like this time:
````
curl -XGET "http://localhost:9200/demo-grok/_search?pretty=true" -H 'Content-Type: application/json' -d'{ }'
````

## Besides the entries we saw the first time, we will now see a sixth entry that looks like this:

tag => _grokparsefailure

````
{
"_index" : "demo-grok",
"_type" : "_doc",
"_id" : "Gjyr9XEBIGK-cCtPRI4L",
"_score" : 1.0,
"_source" : {
"tags" : [
"_grokparsefailure"
],
"@timestamp" : "2020-05-08T19:02:53.768Z",
"path" : "/home/student/03-grok-examples/sample.log",
"message" : "55.12.32.134 GET /user/id/properties",
"@version" : "1",
"host" : "coralogix"
}
````
