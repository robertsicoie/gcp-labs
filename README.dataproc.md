# Cloud Dataproc
Cloud Dataproc should be used for running Spark and Hadoop jobs.

Demo
==
Open Cloud Shell and set desired zone.
```bash
gcloud config set compute/zone europe-west4-a
```
Create a new cluster
```bash
gcloud dataproc clusters create cluster-1 --num-workers=2 --master-machine-type=n1-standard-1 --worker-machine-type=n1-standard-1
```

Run the example job that calculates Pi
```bash
gcloud dataproc jobs submit spark --cluster cluster-1 --class org.apache.spark.examples.SparkPi   --jars file:///usr/lib/spark/examples/jars/spark-examples.jar -- 5
```

This runs the [JavaSparkPi](https://github.com/apache/spark/blob/master/examples/src/main/java/org/apache/spark/examples/JavaSparkPi.java) to demonstrate using Spark to calculate Pi with the [Monte Carlo method](https://academo.org/demos/estimating-pi-monte-carlo/). 

To remove the created cluster run
```bash
gcloud dataproc clusters delete cluster-1
```
Also delete the Cloud Storage buckets created by Dataproc for running the Spark job.
