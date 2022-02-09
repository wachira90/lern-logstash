# lern-logstash
lern-logstash

## start config 

````
.\bin\logstash.bat -f .\config\dev.conf
````

##  reload automatic on save
````
.\bin\logstash.bat -f .\config\dev.conf --config.reload.automatic
````

## run one tine 
````
.\bin\logstash.bat -f .\config\dev.conf --config.test_and_exit
````



## mysql statement
````
SELECT *, UNIX_TIMESTAMP(modification_time) AS unix_ts_in_secs 
FROM es_table 
WHERE (UNIX_TIMESTAMP(modification_time) > :sql_last_value) ORDER BY modification_time ASC
````
