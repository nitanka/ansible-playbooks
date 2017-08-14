<pre>
   '##::::'##:'##:::'##::'######:::'#######::'##:::::::
    ###::'###:. ##:'##::'##... ##:'##.... ##: ##:::::::
    ####'####::. ####::: ##:::..:: ##:::: ##: ##:::::::
    ## ### ##:::. ##::::. ######:: ##:::: ##: ##:::::::
    ##. #: ##:::: ##:::::..... ##: ##:'## ##: ##:::::::
    ##:.:: ##:::: ##::::'##::: ##: ##:.. ##:: ##:::::::
    ##:::: ##:::: ##::::. ######::. ##### ##: ########:
   ..:::::..:::::..::::::......::::.....:..::........::
</pre>

<h3>Objectives</h3>
* Install Mysql
* Changing the default data direcotry of mysql.
* Create the custom data directory if not present.
* Changing the password for root user.
* Disable remote access for root user.
* Create new databases if needed.
* Create new user if needed.

<h3>Supported Platform</h3>
* Ubuntu 16.04
* Ubuntu 14.04

<h3>Role Structure</h3>
<pre>
    ansible-role-mysql
       |- defaults 
            |- main.yml 
       |- handlers
            |- main.yml
       |- tasks
            |- main.yml  
	    |- variables.yml
            |- setup-Debian.yml     
            |- secure.yml
            |- create-database.yml
            |- create-db-user.yml  
       |- templates 
            |- my.cnf.j2
	    |- root-my.cnf.j2
       |- vars
            |- Debian.yml
</pre>

<h3>Dissection of Files</h3>

* defaults/main.yml
  * Contains variables affecting the configuration for Mysql.
  * Most of the variables are having default values usually present in Mysql configuration file. Most of the variables present 
    in the files are self explanatory.
  * The code snippet shown below is an example to define a user.
    ```
     mysql_users: 
     - name: example
       host: 127.0.0.1
       password: secret
       priv: '*.*:USAGE'
    ```
    
    In the above example the passoword can also be a variable present in ansible vault.
  * The code snippet for creating a database is shown below
    ```
     mysql_databases: 
     - name: example
       collation: utf8_general_ci
       encoding: utf8
    ```
     
     The collation and encoding can be changed accordingly.
  
* handlers/main.yml
  * To start the Mysql service the handlers are present
  __Better to use shell module of ansible to start the service as `service service-name start`__
 
* tasks/main.yml
  * Responsible for including all the other scripts which perform the tasks

* tasks/variables.yml
  * This playbook is responsible for setting up the facts on the remote machine. The facts are based on the target system's OS.

* tasks/setup-Debian.yml
  * To install mysql on the server. The steps performed to install Mysql are:
    * To check if mysql already installed.
    * If not installed update the cache.
    * Install mysql package:
      * If OS is Ubuntu 16.04, install mysql-server
      * If OS is less than Ubuntu 16.04, install mysql-server-5.6
    * Delete Innodb log files.
    * Check the status of the service

* tasks/configure.yml
  This playbook is responsible for performing the following tasks:
  * Replace the default my.cnf file present while installing mysql with the my.cnf.j2 file.
  * Verify if the include directory for creating the custom configuration files.
  * Copy the custom configuration file to the include directory.
  * Create the slow query log, if slow query log is enabled. ("mysql_slow_query_log_enabled=true")
  * Creating the custom data directory.
  * Copy the contents of /var/lib/mysql to the custom data directory.
  * Updating the apparmor configuration, to allow to change the chroot of Mysql.
  * Remove the /var/lib/mysql.
  * Create the error log, if new custom log is needed.
  * Set the ownership of the files.
  * Enable mysql service to run at startup.
  * Start the mysql service.

* tasks/secure.yml 
  * Disallow remote login for root.
  * Get the list of host for root.
  * Update the root password. 

* tasks/create-database.yml
  * Copy the my.cnf to the /root directory. The file contains root username and password. The my.cnf which will be put to 
    the remote location is a template root-my.cnf.j2 (for no password access).  
  * Create the database.
  * Cleanup the my.cnf from /root directory.
  
* tasks/create-db-user.yml
  * Copy the my.cnf to the /root directory. The file contains root username and password. The my.cnf which will be put to
    the remote location is a template root-my.cnf.j2 (for no password access).
  * Create the database user.
  * Cleanup the my.cnf from /root directory.

* templates/my.cnf.j2
  * The template for default global configuration file for Mysql.

* templates/root-my.cnf.j2
  * Template to store the root credentials, which will be used to perform passwordless configuration.



<h3>Sample Setup</h3>

 * hosts/inventory file configuration:
   ```
      [mysql]
      192.168.49.111
   ```

 * site.yml
   ```
      - name: setup mysql
        hosts: mysql
        become: true
        roles: 
        - ansible-role-mysql
   ```

<h3>Scope of Improvements</h3>
* Support for RedHat systems.
* Support for Replication.
