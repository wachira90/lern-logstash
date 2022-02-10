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


## Mutate Filter Configuration Options 

Configuration Options
1. coerce – this sets the default value of an existing field but is null

2. rename – rename a field in the event

3. replace – replace the field with the new value

4. update – update an existing field with new value

5. convert – convert the field value to another data type

6. gsub – This config options will find and replace substitutions in strings. This only affects strings or an array of strings.

7. uppercase – this converts a string field to its uppercase equivalent

8. capitalize – this converts a string field to its capitalized equivalent

9. lowercase – convert a string field to its lowercase equivalent

10. strip – remove the leading and trailing white spaces

11. remove

12. split – This splits an array using a separating character, like a comma

13. join – This will join together an array with a separating character like a comma

14. merge – this merges two arrays together or two hashes. Strings automatically convert to arrays, so you can enter an array and a string together and it will merge two arrays. You cannot merge an array with a hash with this option.

15. copy – Copy the an existing field to a second field. When using this option, the original existing field will be erased.

Other options include:

`add_field` – add a new field to the event

`remove_field` – remove an arbitrary field from the event

`add_tag` – add an arbitrary tag to the event

`remove_tag` – remove the tag from the event if present

`id` – add a unique id to the field event

`enable_metric` – This Logstash configuration option will enable or disable metric logging on only this instance. This is a boolean, true or false.

`periodic_flush` – Also a boolean, this calls the filter flush method at regular intervals.
