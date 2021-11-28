# lern-logstash
lern-logstash

## mysql statement
````
SELECT *, UNIX_TIMESTAMP(modification_time) AS unix_ts_in_secs 
FROM es_table 
WHERE (UNIX_TIMESTAMP(modification_time) > :sql_last_value) ORDER BY modification_time ASC
````
