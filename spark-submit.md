# Spark-Submit

## What is Spark-Submit?

`spark-submit` is a command-line tool used to launch Apache Spark applications on a cluster. It allows you to submit your Spark application code to a Spark cluster for execution. You can run your application locally, on a standalone cluster, on YARN, or on Mesos.

## Arguments

The `spark-submit` command accepts a variety of arguments to specify the application configuration, including:

- [`--master`](https://github.com/RahulRoy-rsp/AWS-Essentials/blob/main/spark-submit.md#--master): The master URL for the cluster (e.g., `local`, `yarn`, `mesos`).
- [`--deploy-mode`](https://github.com/RahulRoy-rsp/AWS-Essentials/blob/main/spark-submit.md#--deploy-mode): The deployment mode (`client` or `cluster`).
- `--class`: The entry point for your application (required for Java and Scala applications).
- `--name`: A name for your Spark application.
- `--num-executors`: The number of executors to launch.
- `--executor-memory`: Memory per executor (e.g., `2g`, `512m`).
- `--executor-cores`: Number of cores per executor.
- `--driver-memory`: Memory for the driver.
- `--files`: Comma-separated list of files to be placed in the working directory of each executor.
- `--archives`: Comma-separated list of archives to be extracted into the working directory of each executor.
- [`--conf`](https://github.com/RahulRoy-rsp/AWS-Essentials/blob/main/spark-submit.md#--conf): Arbitrary Spark configuration properties in key=value format.

## Example

Here's an example of how to use `spark-submit` to run a python file stored in a s3 location in your AWS account:
```bash
spark-submit --master yarn --deploy-mode client s3://bucket-name/file-path/python-file.py
```

---

## --master

The `--master` option specifies the master URL for the cluster you want to use. It defines where the Spark application will run. Here are some common values for `--master`:

- `local`: Run Spark locally with one worker thread (ideal for testing).
- `local[K]`: Run Spark locally with K worker threads (e.g., `local[4]` for four threads).
- `local[*]`: Run Spark locally with as many worker threads as logical cores on your machine.
- `yarn`: Connect to a YARN cluster.
- `mesos://host:port`: Connect to a Mesos cluster.

**Example:**
Here is an example when you want to run a python file as a STEP in using your EMR Cluster
```bash
spark-submit --master yarn --deploy-mode client s3://bucket-rr/players/scores.py
```
This will run the **scores.py** file stored in s3 location *s3://bucket-rr/players/scores.py* in your AWS Account.

---

## --deploy-mode

The --deploy-mode option specifies the deployment mode of the Spark driver program. It can have the following values:

- `client`: The driver runs locally where spark-submit is executed. This is ideal for interactive sessions. (logs can't be viewed in a s3 location)
- `cluster`: The driver runs on one of the cluster's worker nodes. This is ideal for production use. (logs can be viewed in a s3 location)

**Example:**

```bash
spark-submit --master yarn --deploy-mode cluster s3://python-file-location
```
This command will run the Spark application on a YARN cluster with the driver running on one of the worker nodes.

---

## --conf
The --conf option allows you to pass arbitrary Spark configuration properties in the form of key-value pairs. This is useful for fine-tuning your Spark job and specifying additional configurations.

**Example:**
```bash
spark-submit --master yarn --deploy-mode cluster --conf spark.executor.memory=4g --conf spark.executor.cores=2 s3://python-file-location
```
In this example, the Spark application is submitted to a YARN cluster with the following configurations:
- spark.executor.memory=4g: Each executor will have 4 GB of memory.
- spark.executor.cores=2: Each executor will use 2 CPU cores.

---

### Running a Spark Script with Arguments

In the tech world, moving from development (dev) to production (PROD) often involves using the same code while changing the environment (`env`). Here's an example of how to run a Spark script with `environment`, `database_name` as arguments.

## Python Script

Below is a sample Python script that creates a Spark session and processes the arguments:

```python
import sys
from pyspark.sql import SparkSession

# Creating Spark session
spark = SparkSession.builder.appName("Arguments").getOrCreate()

# Reading arguments
env = sys.argv[1]
database_name = sys.argv[2]  + "_" + env
table_name = "players_data"

# Building query
query = f"SELECT * FROM {database_name}.{table_name}"

# Executing the query
df = spark.sql(query)
df.show()

# Stopping the Spark session
spark.stop()
```

- As you can see that it needs two arguments from the user: `environment`, `database_name`.
- So, while submitting the job with the python file we also needs to send the arguments.
- The command to do that is as follows:
  ```bash
  spark-submit --master yarn --deploy-mode client s3://bucket-rr/players/scores.py prod players_db
  ```
- When using `spark-submit`, any arguments that are not prefixed with `--` are passed directly to the application and can be accessed via `sys.argv` in your script.
- These arguments should **always be placed at the end of the command**, after all other Spark-specific options.

---

#### Running a python file as a Step in EMR Cluster
- Select `Step type` as **Custom JAR**
- Give a `Name` to the Step
- `JAR Location` as **command-runner.jar**
- `Arguments`: **spark-submit command**
- `Permission`: Select correct **Role** for running the script
- **Submit** the step


**NOTE: This is just one of the ways to run a python script as a step and AWS UI can have changes any time, as its always evolving**

---
