# Spark-Submit

## What is Spark-Submit?

`spark-submit` is a command-line tool used to launch Apache Spark applications on a cluster. It allows you to submit your Spark application code to a Spark cluster for execution. You can run your application locally, on a standalone cluster, on YARN, or on Mesos.

## Arguments

The `spark-submit` command accepts a variety of arguments to specify the application configuration, including:

- `--master`: The master URL for the cluster (e.g., `local`, `yarn`, `mesos`).
- `--deploy-mode`: The deployment mode (`client` or `cluster`).
- `--class`: The entry point for your application (required for Java and Scala applications).
- `--name`: A name for your Spark application.
- `--num-executors`: The number of executors to launch.
- `--executor-memory`: Memory per executor (e.g., `2g`, `512m`).
- `--executor-cores`: Number of cores per executor.
- `--driver-memory`: Memory for the driver.
- `--files`: Comma-separated list of files to be placed in the working directory of each executor.
- `--archives`: Comma-separated list of archives to be extracted into the working directory of each executor.
- `--conf`: Arbitrary Spark configuration properties in key=value format.

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
