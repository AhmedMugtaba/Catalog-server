# Linux Server Configuration

Configuration of a Linux ubuntu distribution to host our web application.

# Server details

Public IP address: 165.227.214.247

SSH port: 2200

Our URL: http://www.earbit.co/

# Server Configuration

## install packages

* update ubuntu server using ```sudo apt-get update```
* upgrade ubuntu server using ```sudo apt-get upgrade```
* install apache using ```sudo apt-get install apache2```
* Install mod_wsgi using 	```sudo apt-get install libapache2-mod-wsgi python-dev```
* enable mod_wsgi ```sudo a2enmod wsgi```

## Add user - grader

* Add user ```grader``` using ```sudo adduser grader```
#### grader password: password

## Allow sudo commands to user grader

* Access the ```/etc/sudoers.d``` with ```sudo ls /etc/sudoers.d```
* Create the file ```grader```
* Use sudo ```nano /etc/sudoers.d/grader``` to change contents of the file to:
```grader ALL=(ALL) NOPASSWD:ALL```
## Set-up SSH keys for user grader
* using ```ssh-keygen``` in the terminal we generate a keygen named udacity_key with no Passphrase.
*  On our VM use mkdir ```.ssh``` then make a file ```touch .ssh/authorized_keys ```
* from our local machine, open and copy the public key using ```cat ~/.ssh/udacity_fsnd_key.pub ```
* open and paste the key to our vm using ```sudo nano .ssh/authorized_keys``` 

#### File Permissions

* ```sudo chmod 700 .ssh```
* ```sudo chmod 644 .ssh/authorized_keys```

## Creating a Flask App
 * move to the /var/www directory using ```cd /var/www```
 * create FlaskApp using ```sudo mkdir FlaskApp```
 * Move inside this directory using ```cd FlaskApp```
 * Create another directory FlaskApp using ```sudo mkdir FlaskApp```
 * clone ```Catolog project``` to ```FlaskApp directory```  
## Install Flask and Virtualenv
* install ```pip``` using ```sudo apt-get install python-pip```
* install ```virtualenv``` using pip ```sudo pip install virtualenv```
* create the ```virtualenv``` using ```sudo virtualenv venv```
* activating virtual environment using ```source venv/bin/activate```
* Install all the dependency packages from ```requirements.txt```
## Configure and Enable a New Virtual Host
* Edit FlaskApp using ```sudo nano /etc/apache2/sites-available/FlaskApp.conf```
* Add the following lines the file 
```<VirtualHost *:80>
		ServerName 165.227.214.247
		ServerAdmin admin@165.227.214.247
		WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
		<Directory /var/www/FlaskApp/FlaskApp/>
			Order allow,deny
			Allow from all
		</Directory>
		Alias /static /var/www/FlaskApp/FlaskApp/static
		<Directory /var/www/FlaskApp/FlaskApp/static/>
			Order allow,deny
			Allow from all
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
* Enable the virtual host using ```sudo a2ensite FlaskApp```

## Create the .wsgi File

* inside the directory ```/var/www/FlaskApp``` create a file ```flaskapp.wsgi```
* cd into ```/var/www/FlaskApp``` and Add 
```#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = 'ahmed_mugtaba_ahmed_ali'
```
## Configure PostgreSQL database
* Install postgres using ```sudo apt-get install postgresql postgresql-contrib```
* Create user ```catalog``` with password 123456
* Create a database managed by ```catalog```
* Connect to the database - ```\c catalog```

## Edits the catalog repository

* change the ```project.py``` to ```__init__.py```
* change from sqlite engine to postgrese 
```engine = create_engine('postgresql://catalog:123456@localhost/catalog')```

### update Google Endpoints

## Restart Apache 

Restart Apache using ```sudo service apache2 restart``` to apply all the changes and launch our app 


## I followed these articles to finish this project 
 * https://www.digitalocean.com/community/tutorials/how-to-create-remove-manage-tables-in-postgresql-on-a-cloud-server
 * https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
 * https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu
 * https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04



