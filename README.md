# cfcf migration - pipeline identification

PS : Please do a ```git clone``` to get an unformatted version of the script file.

This is a sample script to identify all pipelines under a codefresh account impacted by  codefresh integreted registry a.k.a CFCR, deprectation.

More about CFCR registry depreaction - https://codefresh.io/docs/docs/docker-registries/cfcr-deprecation/

## script logic

This script identifies the various pipelines in the following order.

1. Find metadata for all pipelines under an account using the restapi.

  1.1 Get all the meta data for all the pipelines under a codefresh account.
  
  1.2.Convert/Simplify the metadata into meta header and inline pipeline contents only.

2. Iterate over every pipeline metadata record to produce the pipeline yaml/json content
    
     2.1 If a build exists for a pipeline , retrieve the build and extracts YAML from the build into an output file.
      
       2.1.1 Convert the yaml from build into a JSON representaion file.
       
     2.2 If not build exists for a pipeline, save the inline "steps" from the metadata inot pipeline json file.
     
3. Inspect pipeline defintions and idenitfy if CFCR migration impact step exists.
 
     3.1 check for a. build steps using codefresh  type:build
     
     3.2 check for a push steps using codefresh type:push and registry:'cfcr'
     
     3.3 check for pull steps using image registry containing 'r.cfcr.io/*'
     
     collect the pipeline and steps details of those pipelines matching 3.1, 3.2 and 3.3 criteria into an output csv report file.

### How to run the script

Run the script passing the codefresh account ```API key, account name  and a count ``` to limit to restrict the no of repo pipeline to process.

E.g 

```
./cfcrmigidentify.sh api-key cfdemo 10
```

### Output

Script will generate a number pipeline spec files into a directory by name /cfpips.

Along with an execution log and err log files, the important report file that you should look out for is an output csv file containing all the impacted pipelines.

E.g 
```
cfdemo_pipidentification_Report.csv
