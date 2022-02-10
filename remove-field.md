# remove field
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
	remove_field => [ "Password" ] }
}

output {
  stdout { codec => rubydebug }
}
````
