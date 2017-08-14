<pre>
######  ####   ####  #    # ###### ###### #####  ###### #####  
    #  #    # #    # #   #  #      #      #    # #      #    # 
   #   #    # #    # ####   #####  #####  #    # #####  #    # 
  #    #    # #    # #  #   #      #      #####  #      #####  
 #     #    # #    # #   #  #      #      #      #      #   #  
######  ####   ####  #    # ###### ###### #      ###### #    # 
</pre>

<h2>Objectives</h2>
Steps playbook performs.
* Creating the group to run zookeeper. 
* Creating the user to run zookeeper.
* Download zookeeper binaries.
* Extract the binaries and move it to suitable location.
* Creating the log directory, dataDirectory and dataLog directory.
* Set up the zoo.cfg file.
* Set up the myid file.
* Set up the log4j.properties.
* Set up the service script.
* Start the service.

<h2>Supported Platforms</h2>
- Ubuntu 14.04 LTS

<h2>Pre-Requisite</h2>
- ansible-role-java

<h2>Dissection of role</h2>
<pre>
   ansible-role-zookeeper
     |- defaults
     |     |- main.yml
     |- tasks
     |     |- download.yml
     |     |- configure.yml
     |     |- start-service.yml
     |     |- main.yml
     |- templates
           |- zoo.cfg.j2
           |- log4j.properties.j2
           |- zookeeper-service.j2
</pre>
<h2>Dissection of files</h2>
- defaults/main.yml

  * The default variables used for configuring the server.
  
- tasks/download.yml
   
  * Creating the user and group for running zookeeper.
  * Creating the download directories.
  * Downloading and extracting the binaries.
  * Copying the binary to suitable location.
  

- tasks/configure.yml
  
  * Creating the log directories.
  * Copying the zoo.cfg template to the configuration location.
  * Copying the log4j.properties template to the configuration location.
  * Copying the myid template to the configuration location.
  * Copying the service script to the location.
  * Enabling the service script.

- tasks/start-service.yml
  
  * Restart the service in the cluster.

- tasks/main.yml

  * Include all the tasks file by checking the condition if the node is already part of existing zookeeper cluster.

- templates/log4j.properties.j2

  * log4j properties template.

- templates/zoo.cfg.j2
  
  * zookeeper configuration template.

- templates/zookeeper-service.j2

  * Service script for zookeeper service.


