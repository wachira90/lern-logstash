# merg field

````
input {
  file {
    path => "/Users/put/Downloads/Mutate_plugin.CSV"
    start_position => "beginning"
   sincedb_path => "NULL"  }}

filter {
  csv { autodetect_column_names => true }

  mutate {
                merge => { "State" => "City" } }

  mutate {
                rename => [ "State" , "State-City" ]  } }

output {
  stdout { codec => rubydebug }
}
````
