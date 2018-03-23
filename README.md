# Linux server Configuration

## Introduction

This is a project to satify the last project requirement of Udacity Full Stack Nanodegree Program. It aims to setup a virtual linux environment running an apache server that has access to a postgresql database. It is also good to practice about authentication and user management in linux environment. 

## Project Setup

For this project I used Google Cloud VM instances instead of Amazon as I already had an account. I chose Ubuntu 17.10 for this purpose. Google Cloud VM instances has an integrated Firewall and by default it is not allowed to be accessed for HTTP traffic. So that should be allowed in the initial setup. Also the user is not allowed to log in by SSH using password and key usage is enforced but that is OK as it is a requirement in the project.

## Steps taken for the project

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
 ```sudo chmod 700 /home/grader/.ssh  
 sudo chmod 644 /home/grader/.ssh/authorized_keys  
 sudo chown -R grader:grader /home/grader/.ssh```
