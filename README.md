IP address of web app : 13.233.148.2

SSH Port = 2200  

Server alias: http://13.233.148.2.xip.io

Summary of software installed

apache2
libapache2-mod-wsgi
postgresql
git
pip
virtualenv

Python Packages

flask
SQLAlchemy
oauth2client
httplib2
requests
jsonlib
psycopg2


Create a new Server

Movie Catalog Application is deployed on Lightsail instance 
  
  Create a lightsail instance.
  Create a new key from Account >SSH-keys.
  Download the (private)key to SSH in to newly created lightsail instance.

Opening a new SSH Session on your local computer's terminal.

 <code>ssh -i 'key.pem' ubuntu@13.233.148.2 </code>
 
 While logged in ubuntu user run the following commands
 <code>sudo apt update</code>
 <code>sudo apt upgrade</code>
 <code>reboot</code>

Changing the SSH Port from 22 to 2200
<code>nano /etc/ssh/sshd_config</code>
Change line 'Port 22' to 'Port 2200' and save the file

Restart ssh servive
<code>sudo service ssh restart</code>
<code>exit</code>
 
 New login from local machine 
 <code>ssh -i 'key.pem' ubuntu@13.233.148.2 -p 2200 </code>
 
 Configure Timezone to use UTC
 <code>dpkg-reconfigure tzdata</code>
  
Creating new user 'grader' and addding it to sudo Group

<code>sudo adduser grader</code>
<code>usermod -aG sudo grader</code>

Create a file in sudoers.d for user grader
<code>sudo nano /etc/sudoers.d/grader</code>
add `grader ALL=(ALL:ALL) ALL` to file and save

Adding SSH Access to the user 'grader'
Login as user grader to the virtual server
<code></code>


