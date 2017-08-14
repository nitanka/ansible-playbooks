<pre>

:'######::'########:::::'###::::'########::'##:::'##:
'##... ##: ##.... ##:::'## ##::: ##.... ##: ##::'##::
 ##:::..:: ##:::: ##::'##:. ##:: ##:::: ##: ##:'##:::
. ######:: ########::'##:::. ##: ########:: #####::::
:..... ##: ##.....::: #########: ##.. ##::: ##. ##:::
'##::: ##: ##:::::::: ##.... ##: ##::. ##:: ##:. ##::
. ######:: ##:::::::: ##:::: ##: ##:::. ##: ##::. ##:
:......:::..:::::::::..:::::..::..:::::..::..::::..::
</pre>

<h3>Objectives</h3>
Steps playbook performs

- Creating user
- Downloading Spark
- Configuring Spark
- Start master
- Start slave
- Start new slave
- Delete spark tarball files

<h3>Supported Platforms</h3>

 - Ubuntu 16.04
 - Ubuntu 14.04

<h3>Pre-requisite</h3>
 
 - ansible-role-java

<h3>Dissection of Files</h3>

create-user.yaml :

- Creating user for spark directories

download.yaml :

- Download spark tarball file
- Extracting spark tarball file
- Creating Directory for spark Installation
- Changing ownership of Directory

configure.yaml :

- Copying spark-env.sh configuration file to server
- Creating Log directory for spark 

start-spark-master.yaml :

- Starting spark in master node

start-spark-slave.yaml :

- Starting spark in slave nodes

start-spark-new-slave.yaml :

- Starting spark in slave nodes on addition of new node

clean-up.yaml :

- Delete spark tarball files

<h3>Variables</h3>
<pre>
  SPARK_USER = owner user of Installation directory
  SPARK_GROUP = owner group of Installation directory
  SPARK_VERSION = Requied Version of Spark
  HADOOP_VERSION = Required Version of Hadoop
  SPARK_URL = Download URL for spark
  SPARK_HOME = Spark home Directory
  SPARK_MASTER_WEBUI_PORT = UI port of spark
  SPARK_MASTER_EXTERNAL_IP = Public IP of the Spark Master
  SPARK_MASTER_HOST = Private IP of Spark Master 
  SPARK_NEW_NODE = Flag for adding new Node (True or false)
  SPARK_MASTER_PORT = Connector port of Spark
  SPARK_WORKER_CORES = To set the number of cores to use on this machine
  SPARK_WORKER_WEBUI_PORT = UI port of spark worker
  SPARK_WORKER_MEMORY = To set how much total memory workers have to give executors
  SPARK_WORKER_PORT: Connector port of Spark worker
  SPARK_WORKER_INSTANCES = To set the number of worker processes per node
  SPARK_WORKER_DIR = To set the working directory of worker processes
  SPARK_WORKER_OPTS = To set config properties only for the worker (e.g. "-Dx=y")
  SPARK_DAEMON_MEMORY = To allocate to the master, worker and history server themselves (default: 1g).
  SPARK_HISTORY_OPTS = To set config properties only for the history server (e.g. "-Dx=y")
  SPARK_SHUFFLE_OPTS = To set config properties only for the external shuffle service (e.g. "-Dx=y")
  SPARK_DAEMON_JAVA_OPTS = To set config properties for all daemons (e.g. "-Dx=y")
  SPARK_PUBLIC_DNS = To set the public dns name of the master or workers
  SPARK_LOG_DIR = Where log files are stored
  SPARK_PID_DIR = Where the pid file is stored
</pre> 


<h3> Example </h3>

<h5>Setting up new Cluster</h5>

```
  #hosts file/inventory file

  [master]
  192.168.49.11
  [slaves]
  192.168.49.12
  192.168.49.13

```
```
  #file test.yml
  ---
  - name: Spark installation
    hosts: master,slaves
    remote_user: ubuntu
    become: yes
    roles:
    - ansible-role-spark
```
<h5>Adding new slave to the cluster</h5>
The example shown below will show how to add a new slave to the server.

```
  #hosts file/inventory file

  [master]
  192.168.49.11
  [slaves]
  192.168.49.12
  192.168.49.13
  192.168.49.14
  192.168.49.15

```


```
  #file addslave.yml
  ---
  - name: Spark installation
    hosts: master,slaves
    remote_user: ubuntu
    become: yes
    roles:
    - ansible-role-spark
```


