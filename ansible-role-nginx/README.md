<pre>
.__   __.   _______  __  .__   __. ___   ___ 
|  \ |  |  /  _____||  | |  \ |  | \  \ /  / 
|   \|  | |  |  __  |  | |   \|  |  \  V  /  
|  . `  | |  | |_ | |  | |  . `  |   >   <   
|  |\   | |  |__| | |  | |  |\   |  /  .  \  
|__| \__|  \______| |__| |__| \__| /__/ \__\ 
</pre>

<h3>Objectives:</h3>

* Setup the PPA for nginx (reinstall it if PPA got changed)
* Update the cache 
* Set up the nginx configuration
* Setup the virtual host
* Remove the default virtual host
* Link the vhost and enable it
* Restart the nginx 

<h3>Supported Platform</h3>

* Ubuntu 14.04

<h3>Role Structure</h3>
<pre>
   ansible-role-nginx
      |-README.md
      |-defaults
          |-main.yml
      |-tasks
          |- main.yml
          |- setup-Debian.yml  
          |- setup-FreeBSD.yml  
          |- setup-OpenBSD.yml 
          |- setup-RedHat.yml
          |- setup-Ubuntu.yml
          |- vhosts.yml
      |-templates
          |- myhost.j2  
          |- nginx.conf.j2 
          |- nginx.repo.j2  
          |- vhost.j2
      |-vars
          |- Debian.yml  
          |- FreeBSD.yml  
          |- OpenBSD.yml  
          |- RedHat.yml
</pre>      

<h3>Dissection of Files</h3>

* defaults/main.yml
  * Contain the configuration for the nginx.conf as well as the virtual hosts
  * Most of the things we can leave as it it. Since the defaults will be enough to make the server up and running.
  * Based on the preferences we can change the values for 
    * nginx_error_log
    * nginx_access_log
    * nginx_conf_template ( template for nginx.conf )
    * nginx_vhost_template ( templates for vhost files )
    * nginx_extra_http_options ( added in the nginx.conf )
    * nginx_vhosts (it is a list, you can create multiple vhost with same ansible script, just change the underlying values.
      * listen: listen port for the vhost
      * server_name: name of the server
      * root: root location for index file
      * index: index file name (defualt: "index.html index.htm")
            * filename: filename to use in as vhost file name "example.com.conf" ,Can be used to set the filename of the vhost file.
            * Properties that are only added if defined:
    	* server_name_redirect: "www.example.com" # default: N/A
    	* error_page: location of error page
    	* access_log: location of access log
    	* error_log: location of error page
    	* extra_parameters: Can be used to add extra config blocks (multiline). example shown file.
    	* template: Can be used to override the `nginx_vhost_template` per host.
    	* state: "absent" # To remove the vhost configuration.
 
* tasks/main.yml
  * detect the os family, based on the family include the setup file.
  * setup-Ubuntu.yml will be used for Ubuntu platforms.

* tasks/setupUbuntu.yml
  * To install nginx.

* tasks/vhost.yml
  * Setup the nginx.conf file.
  * Setup the vhost template file.
  * Soft link the vhost file from /etc/nginx/sites-availables/<file-name> to /etc/nginx/sites-enabled/

* templates/nginx.conf.j2
  * Template will be used to setup the nginx.conf file.
  * It will be default file for global nginx configuration. 
  * The template will setup the global logs, global configuration for proxy and other global configurations

* templates/vhost.j2
  * To setup the configuration of the virtual host.
  * If we donot want to use this template, put a file configuration file with .j2 extension.
  * The configuration name of the file in remote hosts will be same as the server_name.
  * __If you are giving server name in a custom template file, donot forget to give the server_name in defaults/main.yml also in the section nginx_vhosts__
   
