# Linux server Configuration

## Introduction

This is a project to satify the last project requirement of Udacity Full Stack Nanodegree Program. It aims to setup a virtual linux environment running an apache server that has access to a postgresql database. It is also good to practice about authentication and user management in linux environment. 

## Project Setup

For this project I used Google Cloud VM instances instead of Amazon as I already had an account. I chose Ubuntu 17.10 for this purpose. Google Cloud VM instances has an integrated Firewall and by default it is not allowed to be accessed for HTTP traffic. So that should be allowed in the initial setup. Also the user is not allowed to log in by SSH using password and key usage is enforced but that is OK as it is a requirement in the project.

## Steps taken for the project

### Update server
```
sudo apt-get update
sudo apt-get upgrade
```

### Install and Configure Apache

* Install apache  
`sudo apt-get install apache2`  
* Install mod_wsgi  
`sudo apt-get install libapache2-mod-wsgi python-dev`  
* Enable wsgi  
`sudo a2enmod wsgi`  
* Start Apache  
`sudo service apache2 start`  
By now it should show default index.html page that is installed with apache server.

* Create catalog.wsgi  
`sudo nano catalog.wsgi`  
paste the following into it and save;  
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/")
  
from catalog import app as application
application.secret_key = 'super_secret_key'
```

### Make necessary installation flask etc...
* Install pip for python  
`sudo apt-get install python-pip` 
* Install virtualenv:  
`sudo pip install virtualenv` 
* create a virtual environment  
`sudo virtualenv venv`
* set permissions  
`sudo chmod -R 777 venv` 
* start venv  
`source venv/bin/activate` 
* install flask  
`pip install Flask` 
* install needed plugins 
`pip install httplib2`  
`pip install requests` 
`sudo pip install oauth2client`  
`pip install sqlalchemy`  
`sudo apt-get install python-psycopg2`  

* stop venv  
 `deactivate`

### Virtual host configuration file

* Create configuration file  
`sudo nano /etc/apache2/sites-available/catalog.conf`
* Paste the following  
  ```
   <VirtualHost *:80>
      ServerName 35.187.230.51
      ServerAdmin ozgunbalaban@3.187.230.51
      ServerAlias www.architecturalcomputation.com
      WSGIScriptAlias / /var/www/catalog/catalog.wsgi
      <Directory /var/www/catalog/catalog/>
          Order allow,deny
          Allow from all
      </Directory>
      Alias /static /var/www/catalog/catalog/static
      <Directory /var/www/catalog/catalog/static/>
          Order allow,deny
          Allow from all
      </Directory>
      ErrorLog ${APACHE_LOG_DIR}/error.log
      LogLevel warn
      CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```
  I need to mention, I had a domain name architecturalcomputation.com that I used for this project. As my domain name was through godaddy.com I need to do some configuration. 
  I used the steps here;  https://www.onepagezen.com/transfer-domain-google-cloud-platform/
  after the configuration I could access from the domain name architecturalcomputation.com
 * enable virtual host 
 `sudo a2ensite catalog`
 
### Install 
 
### Clone previous project from Github

* Clone catalog project  
`git clone https://github.com/AlpinistPanda/catalog-app.git` 
* copy the contents to a folder  
`cp -r /catalog-app /var/www/catalog/catalog/`  

### Create a grader user
 * Create a grader user   
 `sudo adduser grader`  
 * Open up the list of sudoers  
 `sudo visudo`
 * Add grader as a sudo   
  insert line below root  
  `grader ALL=(ALL:ALL) ALL`
 * Create a .ssh folder  
 `sudo mkdir /home/grader/.shh` 
 * create an ssh key in the local machine  
 `ssh-keygen`  
 I named the file as udacity and didnt give any password. 
 * create authorized_key  
 `sudo nano /home/grader/.shh/authorized_keys`  
 and paste udacity.pub file content
 * change the ownership of the file  
 ``` 
 sudo chmod 700 /home/grader/.ssh  
 sudo chmod 644 /home/grader/.ssh/authorized_keys  
 sudo chown -R grader:grader /home/grader/.ssh 
 ```
Before this it wasnt possible to ssh to server as Google Cloud needs key based authentication by default after this steps we can log in by;  
`ssh -i udacity grader@architecturalcomputation.com`
* Change ssh port to 2200  
`sudo nano /etc/ssh/sshd_config`  
* Restart ssh service 
`sudo service ssh restart`
* Configure firewall  
 Google cloud has its own configuration for firewall. It should be configured from that place. https://stackoverflow.com/questions/21065922/how-to-open-a-specific-port-such-as-9090-in-google-compute-engine
 I changed the default rule of port 22 for ssh to 2200 for ssh. 
 
 
