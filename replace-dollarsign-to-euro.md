# replace dollarsign to euro

````
 filter {
      mutate {
        gsub => [
          # replace all dollar signs with euros
          "fieldname1", "$", "€",
          # replace backslashes and forward slashes with a hyphen
          "fieldname2", "[\\/]", "-"
        ]
      }
    }
````    
