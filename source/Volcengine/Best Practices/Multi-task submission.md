# Multi-task submission
This best practice is to use PopGenomics CLI on volcengine platform to batch submit `germline variant calling` tasks

## 1. Get the workflow inputs and outputs json
At first we need to get the inputs and outputs json of the workflow, use the command:
```shell
$ popgen vol get-template -w PopGenomics -n germline-variant-calling-gpu
```
You will get two json files including the workflow inputs and outputs.

The outputs json file can be used directly, but the inputs json file nead to add some parameters to the workflow.

The template inputs json file like this:
```json
{
    # if you want to split the GVCF and VCF file according to chromosome
    "germline_variant_calling_gpu.if_split_variant": "Boolean",
    # the tos path to fastq1 file
    "germline_variant_calling_gpu.inputFASTQ_1": "File",
    # the tos path to fastq2 file
    "germline_variant_calling_gpu.inputFASTQ_2": "File",
    # the tos path to the tar file of the reference genome index
    "germline_variant_calling_gpu.inputRefTarball": "File",
    # the tos path to the tar file of the interval file which used to split the GVCF and VCF 
    "germline_variant_calling_gpu.interval_tar": "File",
    # the sample name
    "germline_variant_calling_gpu.sample_name": "String" 
}
```
this is the WDL inputs json for the `germline-variant-calling-gpu` workflow.

In these 6 parameters, `if_split_variant`, `inputRefTarball`, `interval_tar` are the same for each sample, while `inputFASTQ_1`, `inputFASTQ_2`, `sample_name` need to match each sample, so you need to change the inputs json file into this:

```json
{
    "germline_variant_calling_gpu.if_split_variant": true,
    "germline_variant_calling_gpu.inputFASTQ_1": "this.fastq1",
    "germline_variant_calling_gpu.inputFASTQ_2": "this.fastq2",
    "germline_variant_calling_gpu.inputRefTarball": "s3://bucket_name/path/to/ref.fa.tar",
    "germline_variant_calling_gpu.interval_tar": "s3://bucket_name/path/to/ref.interval.tar",
    "germline_variant_calling_gpu.sample_name": "this.sample_name"
}
```
The 3 constant parameters write directly in the json, while the last 3 dynamic parameters need to be read from the **Data model**, so add prefix `this.` to match the column name of the **Data model** columns.


## 2. Data model preparation
We suppose you have already uploaded your raw fastq files on the volcengine TOS (Tinder Object Storage) in **Beijing** region,if not you can follow [tosutil upload data](https://www.volcengine.com/docs/6349/152752). And you need to get all the fastq files tos path(change path prefix from `tos://` to `s3://`) and organize them to a meta data table in `CSV` format like this:
|sample_name|fastq1|fastq2|
|:---:|:--:|:---:|
|sample1|s3://bucket_name/path/to/sample1_1.fastq.gz|s3://bucket_name/path/to/sample1_2.fastq.gz|
|sample2|s3://bucket_name/path/to/sample2_1.fastq.gz|s3://bucket_name/path/to/sample2_2.fastq.gz|
|sample3|s3://bucket_name/path/to/sample3_1.fastq.gz|s3://bucket_name/path/to/sample3_2.fastq.gz|

The rules to organize the meata CSV file are:
1. The CSV file has header line.
2. The column names must include the sufix of parameters in the inputs json file which with `this.` prefix. For example: if you have a parameter `this.sample_name` in the inputs json file, you must have a column name `sample_name` in the CSV file.
3. The elements of the first column of the table must be unique 

## 3. Create data model
Onece you have prepared the data model CSV file for a specific workflow, then you need to create a new data model to input paramters and store the results of the workflow.

```shell
# Create a new data model whose name is 'meta_data'
$ popgen vol create-data-model -w PopGenomics -n germline-variant-calling-gpu -i germline-variant-calling-gpu.inputs.json -m meta_data.csv
```
PopGenomics CLI will check if the meta_data.csv matches the inputs json and the workflow inputs, when there is no error, you will uploaded the data model successfully, and the **data model name** is the file name of the CSV file, this example the data model name is meta_data, you need to record the data model name in case you want to submit analysis tasks with this data model.

## 4. Submit the analysis tasks
With the data model you can submit the analysis tasks, use create-submission sub-command to submit the analysis tasks and polling the task status

```shell
$ popgen vol create-submission -w PopGenomics -n germline-variant-calling-gpu -i germline-variant-calling-gpu.inputs.json -o germline-variant-calling-gpu.outputs.json -m meta_data
```
In this command, the outputs json is used to receive the results of the workflow and write into the data model by adding a new column to the data model, like there is a parameter `outputBAM` in the outputs json, when a workflow runs successfully, the BAM file path will write into the data model **meta_data**'s new column **outputBAM**, be careful if you did not set this parameter in the command line, the result will not be written to the data model.

If you did not set `-l` parameter, the program will run all the samples in the data model by default, if you just want to run some of the samples in the data model or retry the failed samples in the data model, you can input a sample list which is the elements of the csv file's first column, without header line, each element one line, For example:
```
sample1
sample3
```
You can also use the `--webhook` parameter to receive the polling message to feishu robot, so the complete command like this:
```shell
$ popgen vol create-submission \
    -w PopGenomics \
    -n germline-variant-calling-gpu \
    -i germline-variant-calling-gpu.inputs.json \
    -o germline-variant-calling-gpu.outputs.json \
    -m meta_data \
    -l samples.list \
    --webhook https://open.feishu.cn/open-apis/bot/v2/hook/********
```
If submit the analysis tasks successfully, you will get a **submission ID** which is important for you to check the submission status

## 5. Check the running status
When the batch submission is running, the CLI will polling the amount of succeeded, failed and running tasks, you can also use `get-submission` sub-command to get these information if the polling was terminated in accident.

When you want to know each tasks' status in detail, you can use the `get-runs` sub-command 
```shell
$ popgen vol get-runs -w PopGenomics -s submission_id -o output.csv
```

## 6. Get the results
When the submission is finished, you can use the `get-data-model` sub-command to get the results
```shell
$ popgen vol get-data-model -w PopGenomics -m meta_data
```

## Important information
1. you need to record the data model name to submit and get the results
2. you need to record the submssion id to get the running status
