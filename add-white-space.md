# add white space

````
input {
  file {
    path => "/Users/put/Downloads/Mutate_plugin.CSV"
    start_position => "beginning"
   sincedb_path => "NULL"
  }
}

filter {
  csv { autodetect_column_names => true }

  mutate {
	 gsub => [ "message", "," , ", " ]
	 }
 }

output {
  stdout { codec => rubydebug }
}
````

## output
````
a,b,c   = > a, b, c
````
