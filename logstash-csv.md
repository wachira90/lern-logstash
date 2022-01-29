# logstash-csv

````
filter {
  csv {
      separator => ","
      skip_header => "true"
      columns => ["id","timestamp","paymentType","name","gender","ip_address","purpose","country","age"]
  }
}
````


## example 

````
input {
	file {
		path => "/home/ubuntu/Documents/esearch/conn250K.csv"
		start_position => "beginning"
	}
}

filter {
	csv {
		columns => [ "record_id", "duration", "src_bytes", "dest_bytes" ]
	}
}

output {
	elasticsearch { 
		hosts => ["localhost:9200"] 
		index => "mydata-%{+YYYY.MM.dd}"
	}
}
````
