# Function list

## config

Record the user's AK, SK and endpoint parameters, eliminating the need for the user to manually input parameters again when using the main functions

```
$ popgen vol config
Enter ACCESS KEY ID: 
Enter ACCESS KEY SECRET: 
Enter endpoint:
```

current support endpoint in Volcengine is: `cn-beijing`

## create-submission

submit batch workflow tasks

| Argument name(s) | Optional | Default value | Summary                                                      |
| :--------------- | :------- | :------------ | :----------------------------------------------------------- |
| -w, --workspace  | False    |               | the workspace you want to list app                           |
| -n, --app-name   | False    |               | set the name of the workflow                                 |
| -i, --inputs     | False    |               | the inputs Json for the WDL workflow                         |
| -o, --outputs    | True     |               | the outputs Json for the WDL workflow                        |
| -m, -data-model  | True     |               | the data model name including the inputs to workflow         |
| -l , -data-list  | True     |               | the data list contains which row data you want to run in the data model |
| --webhook        | True     |               | the webhook for the message robot                            |
| --interval       | True     | 600           | The interval time for polling                                |

## create-workflow

import workflow from Gitee

| Argument name(s)     | Optional | Default value | Summary                                                      |
| :------------------- | :------- | :------------ | :----------------------------------------------------------- |
| -w, --workspace      | False    |               | the workspace you want to list app                           |
| -n, --app-name       | False    |               | set the name of the workflow                                 |
| -s, --source         | False    |               | The https address of the Gitee repository which stores the WDL script |
| -desc, --description | True     |               | The description of the application                           |
| -t, --tag            | False    |               | The tag of the Gitee repository which stores the WDL script  |
| --token              | False    |               | The token of the Gitee account                               |
| -p, --path           | False    |               | The path of the main WDL in the Gitee repository             |

## create-data-model

create data model for submit batch workflow tasks

| Argument name(s) | Optional | Default value | Summary                                         |
| ---------------- | -------- | ------------- | ----------------------------------------------- |
| -w, --workspace  | false    |               | the workspace you want to list app              |
| -n, --app-name   | false    |               | set the name of the workflow                    |
| -i, --inputs     | false    |               | the inputs json for the WDL workflow            |
| -m, --meta-data  | false    |               | the meta data of the samples you want to submit |



## get-data-model

get data models in the workspace

## get-runs

get submission runs information

## get-submission

get the submission status

## get-template

get the inputs and outputs json template of the workflow

## list-data-models

list data models in the workspace

## list-workflows

list workflows in the workspace

## list-workspaces

list workspaces

## abort-submission

abort the submission

## get-bill

get bill of the submission

