# example case logstash

### shiplog-send.js

```js
var dgram = require('dgram');
var Buffer = require('buffer');

// const message = Buffer.from('Some bytes');
let sms = `timestamp:2023-03-23T13:52:57.370202+07:00 kk:1 ac:READ br:Chrome dev:desktop des1:"dfert" hostname:app02 ip:::ffff:192.168.1.16 level:info os:Windows pid:29610 status:running system:01 time:2023-03-23T06:52:57.369Z user:4042 sysloghost:localhost severity:notice facility:user programname:xxssdd`;

// let port = 41234;

let port = 5044;
let host = '192.168.6.1';

const message = Buffer.Buffer.from(sms);
const client = dgram.createSocket('udp4');
client.send(message, port, host, (err) => {
  client.close();
});
```

### shiplog-listen.js


```js
var dgram = require('dgram');

const server = dgram.createSocket('udp4');

server.on('error', (err) => {
  console.error(`server error:\n${err.stack}`);
  server.close();
});

server.on('message', (msg, rinfo) => {
  console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`);
});

server.on('listening', () => {
  const address = server.address();
  console.log(`server listening ${address.address}:${address.port}`);
});

server.bind(41234);
// Prints: server listening 0.0.0.0:41234

```

### shiplog-multi-send.js

```js
var dgram = require('dgram');
var Buffer = require('buffer');

const buf1 = Buffer.Buffer.from('Some ');
const buf2 = Buffer.Buffer.from('bytes');
const client = dgram.createSocket('udp4');
client.send([buf1, buf2], 41234, (err) => {
  client.close();
});
```

### logstash.conf

```
input {
#	stdin { }	
	udp {
		port => "5044"
		host => "192.168.6.1" #BIND ON ADDRESS 
		queue_size => "2000"
		receive_buffer_bytes => "65536"
		workers => "2"
#		 source_ip_fieldname => "test-node"    
#    codec =>  rubydebug
#		 codec => json

	}
}

filter {
	grok {
		match => {
			"message" => 'timestamp:%{TIMESTAMP_ISO8601:stamp_log} kk:%{INT:version} ac:%{WORD:action} br:%{WORD:browser} dev:%{WORD:device} des1:"%{WORD:des}" hostname:%{WORD:hostname} ip:::ffff:%{IP:ipaddress} level:%{WORD:level} os:%{WORD:os} pid:%{INT:pid} status:%{WORD:sta} system:%{WORD:st} time:%{TIMESTAMP_ISO8601:time_create} user:%{WORD:user} sysloghost:%{HOSTNAME:syshost} severity:%{WORD:severity} facility:%{WORD:facility} programname:%{WORD:prog}'
		}
		remove_field => ["message"]
    }
}

# filter {
# 	mutate {
# 			gsub => [ "message", '{',"" ]
# 			gsub => [ "message", '}',"" ]
# 			gsub => [ "message", '\"',"" ]
# 			gsub => [ "message", '\"',"" ]
# 	}
# }

	# 	mutate {
	# 		remove_field => ["message","path"]
	# #        uppercase => [ "fieldname" ]
	# #        lowercase => [ "fieldname" ]
	# #  	   merge => { "dest_field" => "added_field" }
	#     }
	#}

output {
	# elasticsearch {
	# 	hosts => ["172.16.1.5:9200"]
	# 	action => "index"
	# 	index => "usercsv"
	# }
	stdout { 
		codec =>  rubydebug {
			metadata => true
		}
	}
}
```

### result

```
{
       "severity" => "notice",
             "st" => "01",
      "ipaddress" => "192.168.1.16",
    "time_create" => "2023-03-23T06:52:57.369Z",
            "sta" => "running",
             "os" => "Windows",
          "level" => "info",
            "pid" => "29610",
        "version" => "1",
           "prog" => "xxssdd",
      "stamp_log" => "2023-03-23T13:52:57.370202+07:00",
       "hostname" => "app02",
     "@timestamp" => 2023-03-24T16:01:39.127Z,
            "des" => "dfert",
        "browser" => "Chrome",
       "@version" => "1",
           "host" => "192.168.6.1",
        "syshost" => "localhost",
         "action" => "READ",
       "facility" => "user",
         "device" => "desktop",
           "user" => "4042"
}
```
