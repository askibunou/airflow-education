## 1. Simple DAG

File name: ```{$user_id}_simple_dag```

Tag: ```{$user_id}```, ``task-1``

Owner: ```{$user_id}```

1. Create a DAG with ID - ```{$user_id}-simple-dag```;
2. Create Python/Bash operators with a function ```sleep(10)```;
3. Link operators in composition: D1 --> D2,D3 --> D4,D5,D6,D7 --> D8;
4. Use Dummy operator to separate parallel executions;
5. Ensure that no more than two tasks are executed at the same time.


## 2. Composition from configuration

File name: ```{$user_id}_composition_c1```

Tag: ```{$user_id}```, ``task-2``

Owner: ```{$user_id}```

1. Create a DAG with ID - ```{$user_id}-composition-c1```;
2. Create a config file based on an example:

   ```
   operators = {
    "s1": {
        "up_stream": "Start",
        "down_stream": "",
     },
    "s2": {
        "up_stream": "s1",
        "down_stream": "s4",
     },
    "s3": {
     "up_stream": "s1",
     "down_stream": "s4",
    },
    "s4": {
     "up_stream": "",
     "down_stream": "Finish",
    }
   } 
    ```
   
3. Ensure DAG composition is created from config file for ```{$user_id}_simple_dag```.
-----------
File name: ```{$user_id}_composition_c2```

Tag: ```{$user_id}```, ``task-2``

Owner: ```{$user_id}```

1. Create a DAG with ID - ```{$user_id}-composition-c2```;
2. Create a config file based on an example:

   ```
   operators = {
    "s1": {
        "up_stream": ["Start"],
        "down_stream": ["s2", "s3"],
     },
    "s2": {
        "up_stream": [],
        "down_stream": [],
     },
    "s3": {
     "up_stream": [],
     "down_stream": [],
    }
    "s4": {
     "up_stream": ["s2", "s3"],
     "down_stream": ["Finish"],
    }
   } 
    ```
   
3. Ensure DAG composition is created from config file for ```{$user_id}_simple_dag```.


## 3. Scheduling

File name: ```{$user_id}_schedule```

Tag: ```{$user_id}```, ``task-3``

Owner: ```{$user_id}```

1. Create a DAG with ID - ```{$user_id}-schedule```;
2. Create multiple sequential operators;
3. DAG should run every 10 minutes;
4. The beginning of DAG`s run in the present.
5. Explore options: ```catchup```, ```start_date```, ```schedule_interval``` and ```max_active_runs```


## 4. Working with Variables, XCOM

File name: ```{$user_id}_variables_xcom```

Tag: ```{$user_id}```, ``task-4``

Owner: ```{$user_id}```

Variables: ```{$user_id}-{$any_name}```

1. Create a DAG with ID - ```{$user_id}-variables-xcom```;
2. Create 3 Python operators executed sequentially;
3. Python operators call the ```sleep(180)``` function and output the time;
4. This time must be the same in all operators. Use Variables and XCOM;
5. Explore Options: ```retries```, ```retry_delay```;
6. The second operator should generate an error on the first run and run without errors on the second;
7. Set a delay before restarting the second operator to 2 minutes;
8. Check variables in UI Airflow.


## 5. Introduction to the UI Airflow

Get to know the interface. Answer the questions. How to read DAG Runs, Recent Tasks? How to read logs from the last launch and from the N-th. Deal with task statuses. Explain the convenience of Tree View/Graph views. How to switch to Graph View for N-th run. Explore Details tab. How to use Task Duration, Task Tries, Landing Times, Gant, Code. Explain the hover over window over tasks. Learn how to restart the DAG from a specific task, for example, in step 1, repeat the last run starting from the second operator. Learn to cancel the DAG execution, for example, in step 1, for task 2, set the Failed status during its Sleep 180 (pay attention to how subsequent tasks will react)


## 6. Ingest

File name: ```{$user_id}_ingest_fact, {$user_id}_ingest_dimention```

Tag: ```{$user_id}```, ``task-6``

Owner: ```{$user_id}```

Variables: ```{$user_id}-{$any_name}```

1. Create a DAGs with ID:
   1. ```{$user_id}-ingest-fact```;
   2. ```{$user_id}-ingest-dimension```.
2. Explore SparkOperator;
3. Explore Spark Jobs:
   1. ```covid```;
   2. ```geography```;
   3. ```time_period```.
4. Create ```fact``` and ```dimension``` configs based on ``` {$user_id}-composition ```;
5. Add required parameters for ```covid``` job to ```fact``` config:
    ```python
     "SOURCE_PATH": "https://covid.ourworldindata.org/data/owid-covid-data.json",
     "TARGET_PATH": "cos://airflow-education.airflow/{$user_id}/ingest/{$table}/yyyy-mm-dd",
     "WRITE_MODE": "overwrite",
     "NUM_REPARTITION": 5,
     "SCHEMA": [
                {
                    "metadata": {},
                    "parquet_name": "iso_code",
                    "json_name": "iso_code",
                    "nullable": True,
                    "type": "string"
                },
                {
                    "metadata": {},
                    "parquet_name": "location",
                    "json_name": "location",
                    "nullable": True,
                    "type": "string"
                },
                {
                    "metadata": {},
                    "parquet_name": "date",
                    "json_name": "date",
                    "nullable": True,
                    "type": "date"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_cases",
                    "json_name": "new_cases",
                    "nullable": True,
                    "type": "float"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_cases_smoothed",
                    "json_name": "new_cases_smoothed",
                    "nullable": True,
                    "type": "float"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_cases_per_million",
                    "json_name": "new_cases_per_million",
                    "nullable": True,
                    "type": "float"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_cases_smoothed_per_million",
                    "json_name": "new_cases_smoothed_per_million",
                    "nullable": True,
                    "type": "float"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_deaths_smoothed",
                    "json_name": "new_deaths_smoothed",
                    "nullable": True,
                    "type": "float"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_deaths_smoothed_per_million",
                    "json_name": "new_deaths_smoothed_per_million",
                    "nullable": True,
                    "type": "float"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_vaccinations_smoothed",
                    "json_name": "new_vaccinations_smoothed",
                    "nullable": True,
                    "type": "float"
                },
                {
                    "metadata": {},
                    "parquet_name": "new_vaccinations_smoothed_per_million",
                    "json_name": "new_vaccinations_smoothed_per_million",
                    "nullable": True,
                    "type": "float"
                }
            ]
    ```
6. Add required parameters for ```geography``` and ```time_period``` job to ```dimension``` config:
   1. ```python
         "SOURCE_PATH": "https://raw.githubusercontent.com/skibunov/airflow-education/master/IBM%20Geography.json",
         "TARGET_PATH": "cos://airflow-education.airflow/{$user_id}/ingest/{$table}/yyyy-mm-dd",
         "WRITE_MODE": "overwrite",
         "NUM_REPARTITION": 5,
         "SCHEMA": [
             {
                 "metadata": {},
                 "parquet_name": "shortDescription",
                 "json_name": [
                     "shortDescription"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "mediumDescription",
                 "json_name": [
                     "mediumDescription"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "longDescription",
                 "json_name": [
                     "longDescription"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "version",
                 "json_name": [
                     "version"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "createUserKey",
                 "json_name": [
                     "createUserKey"
                 ],
                 "nullable": True,
                 "type": "short"
             },
             {
                 "metadata": {},
                 "parquet_name": "updateUserKey",
                 "json_name": [
                     "updateUserKey"
                 ],
                 "nullable": True,
                 "type": "short"
             },
             {
                 "metadata": {},
                 "parquet_name": "rowCreateTs",
                 "json_name": [
                     "rowCreateTs"
                 ],
                 "nullable": True,
                 "type": "timestamp"
             },
             {
                 "metadata": {},
                 "parquet_name": "rowUpdateTs",
                 "json_name": [
                     "rowUpdateTs"
                 ],
                 "nullable": True,
                 "type": "timestamp"
             },
             {
                 "metadata": {},
                 "parquet_name": "comments",
                 "json_name": [
                     "comments"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "stdKey",
                 "json_name": [
                     "stdKey"
                 ],
                 "nullable": True,
                 "type": "integer"
             },
             {
                 "metadata": {},
                 "parquet_name": "stdHrchyKey",
                 "json_name": [
                     "stdHrchyKey"
                 ],
                 "nullable": True,
                 "type": "short"
             },
             {
                 "metadata": {},
                 "parquet_name": "stdNodeKey",
                 "json_name": [
                     "stdNodeKey"
                 ],
                 "nullable": True,
                 "type": "short"
             },
             {
                 "metadata": {},
                 "parquet_name": "levelCode",
                 "json_name": [
                     "levelCode"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "levelName",
                 "json_name": [
                     "levelName"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "parentCode",
                 "json_name": [
                     "parentCode"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "parentDescription",
                 "json_name": [
                     "parentDescription"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "code",
                 "json_name": [
                     "code"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "code_type",
                 "json_name": [
                     "nodeCustomData",
                     "properties",
                     "CODE_TYPE"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "compliance_cd",
                 "json_name": [
                     "nodeCustomData",
                     "properties",
                     "COMPLIANCE_CD"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "usage_rule",
                 "json_name": [
                     "nodeCustomData",
                     "properties",
                     "USAGE_RULE"
                 ],
                 "nullable": True,
                 "type": "string"
             },
             {
                 "metadata": {},
                 "parquet_name": "recordStatus",
                 "json_name": [
                     "recordStatus"
                 ],
                 "nullable": True,
                 "type": "string"
             }
         ]
      ```
   2. ```python
        "SOURCE_PATH": "cos://airflow-education.airflow/raw/TIME_PERIOD",
        "TARGET_PATH": "cos://airflow-education.airflow/{$user_id}/ingest/{$table}/yyyy-mm-dd",
        "WRITE_MODE": "overwrite",
        "NUM_REPARTITION": 5,
        "SCHEMA": {
                "type": "struct",
                "fields": [
                    {
                        "metadata": {},
                        "name": "SURROGATE_KEY",
                        "type": "integer",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "WEEK_IN_QUARTER_YEAR_KEY",
                        "type": "integer",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "MONTH_IN_YEAR_KEY",
                        "type": "integer",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "QUARTER_IN_YEAR_KEY",
                        "type": "integer",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "HALF_IN_YEAR_KEY",
                        "type": "integer",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "DATE",
                        "type": "date",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "DAY_NUM",
                        "type": "short",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "DAY_NAME",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "WEEK_IN_QUARTER_NUM",
                        "type": "short",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "WEEK_IN_QUARTER_NAME",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "WEEK_IN_QUARTER_SHORT",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "WEEK_IN_QUARTER_YEAR",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "WEEK_IN_YEAR",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "MONTH_NUM",
                        "type": "short",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "MONTH_SHORT",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "MONTH_IN_YEAR",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "QUARTER_NUM",
                        "type": "short",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "QUARTER_SHORT",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "QUARTER_IN_YEAR",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "HALF_NUM",
                        "type": "short",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "HALF_SHORT",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "HALF_IN_YEAR",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "YEAR_NUM",
                        "type": "short",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "YEAR_SHORT",
                        "type": "string",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "WEEK_BLACKOUT",
                        "type": "boolean",
                        "nullable": True
                    },
                    {
                        "metadata": {},
                        "name": "MONTH_BLACKOUT",
                        "type": "boolean",
                        "nullable": True
                    }
                ]
            }
      
      ```
7. Implement DAG building based on configuration.
8. DAGs must initialize ```yyyy-mm-dd``` in TARGET_PATH;
9. The time must be the same in all operators;
10. If an error occurs, the operator must restart after 5 minutes;
11. Implement a check so that DAG cannot start a second time while the first one is running;

## 7. Validation

File name: ```{$user_id}_validation```

Tag: ```{$user_id}```, ``task-7``

Owner: ```{$user_id}```

Variables: ```{$user_id}-{$any_name}```

1. Create a DAG with ID - ```{$user_id}-validation```;
2. Explain how a ```validation``` job works;
3. Create ```validation```config based on ``` {$user_id}-composition ```;
4. Add required parameters for ```each table``` in the validation config;
5. Implement DAG building based on configuration.


## 8. Transformation


## 9. Trigger

File name: ```{$user_id}_trigger```

Tag: ```{$user_id}```, ``task-9``

Owner: ```{$user_id}```

Variables: ```{$user_id}-{$any_name}```

1. Create a DAG with ID - ```{$user_id}-trigger```;
2. Modify ```ingest``` Dag so that it writes a flag variable indicating that ```ingest``` has ended.
3. In the DAG, use the Sensor operator that reacts to the flag variable: 
   1. if the flag is not set, then wait; 
   2. if it is set, perform ```validation``` and ```transformation``` sequentially;
   3. if a timeout occurs and the flag is not set, skip ```validation``` and ```transformation```.


## 10. Trigger (option with task groups)

File name: ```{$user_id}_trigger_with_task_groups```

Tag: ```{$user_id}```, ``task-10``

Owner: ```{$user_id}```

Variables: ```{$user_id}-{$any_name}```

1. Create a DAG with ID - ```{$user_id}-trigger-with-task-groups```;
2. Combine statements from ```transformation``` and ```validation``` into one DAG;
3. Add groups to relevant operators.


