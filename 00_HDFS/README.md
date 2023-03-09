## HDFS

Go to command line / terminal and run in a folder of your liking:  
`git clone https://github.com/big-data-europe/docker-hadoop`

Replace the Docker compose file in `docker-hadoop` folder with the one in this repo. This will add additional datanodes and a Docker volume for mounting text files to the namenode.  

Then, when in the new docker-hadoop folder, start the Docker services:  
`docker compose up -d`  

Verify that all services started correctly:  
`docker ps`  

You can go into a specific service:  
`docker exec -it namenode bash`  

Create a text file in the `tmp` folder on your local machine and add some words.  

Change to the `/tmp/files` directory on namenode.  
`cd /tmp/files`  

## Hadoop Map Reduce
Hadoop File System docs:  
https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html  

Create a directory for hadoop input  
`hadoop fs -mkdir -p /user/hadoop/input`  

Put the text file into hadoop file system:  
`hadoop fs -put file:/tmp/files/test.txt hdfs:/user/hadoop/input/`  

Check that the file exists:  
`hadoop fs -ls /user/hadoop/input/`  

You can also check that the file exists from the Hadoop UI in your browser:  
`localhost:9870`  

Let's try executing a Hadoop job:  
`cd $HADOOP_HOME`  

`hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount /user/hadoop/input /user/hadoop/output`  

You can change into a datanode and verify that the HDFS files are still visible  
`docker exec -it datanode3 bash`  

Where are the files actually stored? Look into the following folder:  
`/hadoop/dfs/data`  

Alternative CLI:  
`hdfs dfs -ls /`  

The `hdfs` command was designed only for talking with the hdfs file system, while `hadoop` allows you to talk also with the local file system.  

Yarn is the resource manager for Hadoop. We can check the UI:  
`localhost:8088`  

Let's create a new text file in local `tmp` folder to go through the flow once more.  

`docker exec -it namenode  bash`  
`hadoop fs -mkdir /user/hadoop/input2`  
`hadoop fs -put file:/tmp/files/file.txt hdfs:/user/hadoop/input2/`  
`hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount /user/hadoop/input2 /user/hadoop/output2`  

You can view the execution in the yarn UI.  


-----

# Databricks setup

Once the cluster is up and running, we can view both the local file system and dbfs (hdfs):  
`%fs ls`  

`%sh ls`  