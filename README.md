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
 
 
