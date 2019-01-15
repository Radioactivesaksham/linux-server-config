<b>Note:</b> I have worked with two keys here one('key.pem') for logging in to my virtual server with user ubuntu and 2nd I created on my local computer <code>ssh-keygen</code> and placed it ssh directory of user grader. Private key('sailfish') of user grader will be shared in notes to instructor section. 

IP address of web app : 13.233.148.2

SSH Port = 2200  

Server alias: http://13.233.148.2.xip.io

##List of software installed
<ul>
<li>apache2</li>
<li>libapache2-mod-wsgi</li>
<li>postgresql</li>
<li>git</li> 
<li>pip</li>
<li>virtualenv</li>
<li>
Python Packages
<ul>
<li>flask</li>
<li>SQLAlchemy</li>
<li>oauth2client</li>
<li>httplib2</li>
<li>requests</li>
<li>jsonlib</li>
<li>psycopg2</li>
</ul>
</li>
</ul> 

 <b>Movie Catalog Application is deployed on Lightsail instance</b><br> 
  Create a lightsail instance.<br>
  Create a new key from Account >SSH-keys.<br>
  Download the (private)key to SSH in to newly created lightsail instance.<br>

<b>Opening a new SSH Session on your local computer's terminal.</b><br>
<code>ssh -i 'key.pem' ubuntu@13.233.148.2 </code><br>
 While logged in as ubuntu user run the following commands<br>
 <code>sudo apt update</code><br>
 <code>sudo apt upgrade</code><br>
 <code>reboot</code><br>

<b>Changing the SSH Port from 22 to 2200</b><br>
<code>nano /etc/ssh/sshd_config</code><br>
Change line 'Port 22' to 'Port 2200' and save the file.<br> 
Dont forget to allow TCP port 2200 in lightsail intance. Click on manage>networking>firewall. Add Port 2200(TCP)<br>

<b>Restart ssh servive</b><br>
<code>sudo service ssh restart</code><br>
<code>exit</code><br>
 
 <b>New login from local machine</b><br> 
 <code>ssh -i 'key.pem' ubuntu@13.233.148.2 -p 2200 </code><br>
 
 <b>Configure Timezone to use UTC</b><br>
 <code>dpkg-reconfigure tzdata</code><br>
  
<b>Creating new user 'grader' and addding it to sudo Group</b><br>
<code>sudo adduser grader</code><br>
<code>usermod -aG sudo grader</code><br>

<b>Create a file in sudoers.d for user grader</b><br>
<code>sudo nano /etc/sudoers.d/grader</code><br>
Add `grader ALL=(ALL:ALL) ALL` to file and save<br>

<b>Adding SSH Access to the user 'grader'</b><br>
Login as user grader to the virtual server<br>
<code>su -grader</code><br>

<b>Now enter the following commands</b><br>
<code>mkdir .ssh</code><br>
<code>chmod 700 .ssh</code><br>
<code>touch .ssh/authorized_keys</code><br>
<code>nano .ssh/authorized_keys</code><br>
Now pasting the contents of('sailfish') public key in the above file.<br> 
<code>chmod 644 authorized_keys</code>

<b>After this,  run exit and try logging back again as user grader</b><br>
<code>ssh -i 'sailfish' grader@13.233.148.2 -p 2200</code>

<b>Setting up the firewall</b><br>
<code>sudo ufw allow 2200/tcp</code><br>
<code>sudo ufw allow 123/udp</code><br>
<code>sudo ufw allow 80/tcp</code><br>
<code>sudo ufw enable</code>

<b>Setup to install apache2,mod_wsgi and git:</b><br>
To install the Apache Web Server, run the following command after logging in as the 'grader' user via SSH:<br>
<code> sudo apt-get update</code><br>
<code>sudo apt-get install apache2</code><br>
<code> sudo apt-get install libapache2-mod-wsgi python-dev</code><br>
<code> sudo a2enmod wsgi</code><br>
<code>sudo service apache2 start</code><br>
<code>sudo apt-get install git</code><br>

<b>Configure Apache to serve a Python mod_wsgi application.</b>
<code> cd /var/www</code><br>
<code>sudo mkdir catalog </code><br>
<code> sudo chown -R grader:grader catalog</code><br>
<code> cd catalog </code><br>
<code> git clone https://github.com/___</code>

<b>Install pip , setup virtual environment and also install Flask and its dependencies:</b><br>
Install pip, virtualenv (in /var/www/catalog)<br>
<code>sudo apt-get install python-pip</code><br>
<code>sudo pip install virtualenv</code><br>
<code>source venv/bin/activate</code><br>
<code>sudo chmod -R 777 venv</code><br>

<b>Install flask and other dependencies:</b><br>
<code>sudo pip install -r catalog/requirements.txt</code>

<b>Install Python's PostgreSQL adapter psycopg2:</b><br>
<code>sudo apt-get install python-psycopg2</code>

<b>Configure and Enable a New Virtual Host</b><br>
<code>sudo nano /etc/apache2/sites-available/catalog.conf</code><br>
Add the following content in it:

```
<VirtualHost *:80>
   ServerName 13.233.148.2
   ServerAlias 13.233.148.2.xip.io
   ServerAdmin grader@13.233.148.2
   WSGIDaemonProcess __init__ python-path=/var/www/catalog/application:$
   WSGIProcessGroup __init__
   WSGIScriptAlias / /var/www/catalog/catalog.wsgi
   <Directory /var/www/catalog/application/>
       Order allow,deny
       Allow from all
   </Directory>
   Alias /static /var/www/catalog/application/static
   <Directory /var/www/catalog/application/static/>
       Order allow,deny
       Allow from all
   </Directory>
   ErrorLog ${APACHE_LOG_DIR}/error.log
   LogLevel warn
   CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

<b>Enable the new virtual host</b><br>
<code>sudo a2ensite catalog</code>

<b>Create and configure the .wsgi file</b><br>
<code>cd /var/www/catalog/</code><br>
<code>sudo nano catalog.wsgi</code><br>
Add the following content:

```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/application")

from project import app as application
application.secret_key = 'secret'
```

<b>Update the absolute path of client_secrets.json in  project.py</b><br>

<b>Installing and Configuring PostgreSQL</b><br>
<code>sudo apt-get install libpq-dev python-dev</code><br>
<code>sudo apt-get install postgresql postgresql-contrib</code> <br>
<code> sudo -u postgres psql</code> 
 
 ```
1. Create a new user called 'catalog' with his password:``` # CREATE USER catalog WITH PASSWORD 'catalog';```
2. Give catalog user the CREATEDB permission: ```# ALTER USER catalog CREATEDB;```
3. Create the 'catalog' database owned by catalog user: ```# CREATE DATABASE catalog WITH OWNER catalog;```
4. Connect to the database: ```# \c catalog```
5. Revoke all the rights:``` # REVOKE ALL ON SCHEMA public FROM public;```
6. Lock down the permissions to only let catalog role create tables:``` # GRANT ALL ON SCHEMA public TO catalog;```
7. Log out from PostgreSQL:``` # \q```. Then return to the grader user:``` $ exit.```
8. Edit the lotsofitems.py and database.py file:
9. Change engine = create_engine('sqlite:///menu.db') to engine = create_engine('postgresql://catalog:catalog@localhost/catalog')
10. Remote connections to PostgreSQL should already be blocked. Double check by opening the config file:
```
 <b>Restart apache server</b><br>
 <code>sudo service apache2 restart</code><br>
 You will now be able to run your application at http://13.233.148.2
 
<b> Final Updates:</b> <br>
To get the Google login working there are a few additional steps:
1. Go to Google Dev Console<br>
2. Sign up or Login if prompted<br>
3. Go to Credentials<br>
4. Select Create Crendentials > OAuth Client ID<br>
5. Select Web application<br>
6. Enter name 'Item-Catalog'<br>
7. Authorized JavaScript origins = 'http://13.233.148.2.xip.io'<br>
8. Authorized redirect URIs = 'http://13.233.148.2.xip.io/login' && 'http://13.233.148.2.xip.io/gconnect'<br>
9. Select Create<br>
10. Copy the Client ID and paste it into the data-clientid in login.html<br>
11. On the Dev Console Select Download JSON<br>
12. Rename JSON file to client_secrets.json<br>
13. Place JSON file in Udacity-item-catalog directory that you cloned from here.<br>
 
 
 ## References

[1] <https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04>

[2] <http://terokarvinen.com/2016/deploy-flask-python3-on-apache2-ubuntu>

[3] <https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps>

[4] <https://askubuntu.com/questions/293426/system-monitoring-tools-for-ubuntu>

[5] <https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps>




 
 
 
 
