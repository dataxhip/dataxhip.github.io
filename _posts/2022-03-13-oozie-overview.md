# Oozie Overview and foundation


Oozie is a workflow scheduler system to manage Apache Hadoop jobs.

Oozie Workflow jobs are Directed Acyclical Graphs (DAGs) of actions.

Oozie Coordinator jobs are recurrent Oozie Workflow jobs triggered by time (frequency) and data availability.

Oozie is integrated with the rest of the Hadoop stack supporting several types of Hadoop jobs out of the box (such as Java map-reduce, Streaming map-reduce, Pig, Hive, Sqoop and Distcp) as well as system specific jobs (such as Java programs and shell scripts).

Oozie is a scalable, reliable and extensible system.

[For more information go there](https://oozie.apache.org/)

After configuring oozie with Hadoop Cluster, we need to modify or adapt the job.properties file.

The job.properties  file should accessible from the local machine.

```bash
oozie job -oozie http://localhost:11000/oozie -config examples/apps/map-reduce/job.properties -run

job: 14-20090525161321-oozie-tucu
```

This command will return the Oozie job application Id. We could therefore check the workflow of the job


```bash

oozie job -oozie http://localhost:11000/oozie -info 14-20090525161321-oozie-tucu
```

It can be done too this way with the export of OOZIE_URL

```bash
export OOZIE_URL="http://localhost:11000/oozie"

oozie job -info 14-20090525161321-oozie-tucu
```


## Some more practical examples

Considering that we already have Oozie configuration file, the `oozie-examples.tar.gz`, we can extract it

```bash
cd /opt/mapr/oozie/oozie-<version>
tar xvfz ./oozie-examples.tar.gz -C /opt/mapr/oozie/oozie-<version>/
```

Copy the examples to filesystem. Run the following command as a user that has permissions to write to the specified filesystem directory.

```bash
hadoop fs -put examples maprfs:///user/<user_name>/examples
```

Now we can run a job or at least the examples job

Choose an example and run it with the oozie job command. 
You can use the following commands to run the following examples. The``< oozie_port_number >`` depends on whether your cluster is secure.
* For a MapReduce Jobs

```bash
./bin/oozie job -oozie="http://localhost:<oozie_port_number>/oozie" -config ./examples/apps/map-reduce/job.properties -run
```

* For Spark Jobs
```bash
./bin/oozie job -oozie="http://localhost:<oozie_port_number>/oozie" -config ./examples/apps/spark/job.properties -run
```


**Note**: To run the packaged Hive examples, make /tmp on filesystem world-writable. Set /tmp to 777. Example:
```bash
hadoop fs -chmod -R 777 /tmp
```
If ``/tmp`` does not exist, create ``/tmp`` and then set it to ``777``.


We can check the status of a job from the command line or the Oozie Web Console.
From the command line, issue the following (substituting the job ID for the <job id> placeholder):

```bash
./bin/oozie job -info <job id>
```
To check status from the Oozie Web Console, point your browser to http://<Oozie_node>:<oozie_port_number>/oozie and click All Jobs. The <oozie_port_number> depends on whether your cluster is secure.


[Here Link to configure a Hive Job with Oozie](https://docs.datafabric.hpe.com/62/Oozie/RunHiveJobswithOozie.html)


[Here Link to configure a Spark Job with Oozie](https://docs.datafabric.hpe.com/62/Oozie/RunSparkJobswithOozie.html)


[Here we can more command line tols for Oozie] https://oozie.apache.org/docs/3.1.3-incubating/DG_CommandLineTool.html


[Cloudera Link to configure Oozie on a Secured Cluster](https://docs.cloudera.com/runtime/7.2.8/configuring-oozie/oozie-managing-hadoop-jobs.pdf)
