# Function list

## config

Record the user's AK, SK, endpoint and region parameters, eliminating the need for the user to manually input parameters again when using the main functions

```
$ popgen vol-pipe config
Enter ACCESS KEY ID: 
Enter ACCESS KEY SECRET: 
Enter region:
Enter TOS endpoint:
```

current support `region` in Volcengine is: `cn-beijing`

current support `TOS endpoint` in Volcengine is: `tos-cn-beijing.volces.com`, if your client is using a server in Volcengine Beijing, the endpoint is `tos-cn-beijing.ivolces.com`

## copy-joint-res-meta

copy joint result file from meta table

| Argument name(s)          | Optional | Default value | Summary                                                      |
| ------------------------- | -------- | ------------- | ------------------------------------------------------------ |
| -i, --input-joint-res     | false    |               | input joint result meta                                      |
| -b,  --target-bucket-path | false    |               | target bucket path to copy split gvcf, tos://bucket_name/path/ |
| -c, --column-list         | false    |               | column names of files to be copied, separated by commas      |
| -t, --threads             | false    | 8             | multi threads to copy TOS file                               |



## delete-file

delete file with submission ID

| Argument name(s)      | Optional | Default value | Summary                              |
| --------------------- | -------- | ------------- | ------------------------------------ |
| -w, --workspace       | false    |               | the workspace you created submission |
| -s, --submission-id   | true     |               | the submission ID to request         |
| -l, --submission-list | true     |               | the submission ID list to request    |



## get-germline-meta

get vol get-data-model input from fastq TOS path

| Argument name(s)  | Optional | Default value | Summary                                                 |
| ----------------- | -------- | ------------- | ------------------------------------------------------- |
| -b, --bucket-path | false    |               | TOS path of fastq files, tos://bucket_name/path/to/file |
| -o, --output      | false    |               | output file name of get-data-model                      |



## get-joint-meta

get vol get-data-model input from variant calling result meta

| Argument name(s)         | Optional | Default value | Summary                                                      |
| ------------------------ | -------- | ------------- | ------------------------------------------------------------ |
| -i, --input-calling-res  | false    |               | input variant calling result meta                            |
| -o, --output-prefix      | false    |               | output file name prefix to joint meta                        |
| -b, --target-bucket-path | false    |               | target bucket path to copy split gvcf, tos://bucket_name/path/ |
| -t, --threads            | false    | 8             | multi threads to copy TOS file                               |



## pop-variant-calling

multi sample variant calling and joint calling pipeline

| Argument name(s) | Optional | Default value | Summary                          |
| ---------------- | -------- | ------------- | -------------------------------- |
| -c, --config     | false    |               | input config file in json format |
| -o, --outdir     | false    |               | output dir path                  |

