IP address of web app : 13.233.148.2

SSH Port = 2200  

Server alias: http://13.233.148.2.xip.io

Summary of software installed

apache2<br>
libapache2-mod-wsgi<br>
postgresql<br>
git<br>
pip<br>
virtualenv<br>

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
 
 While logged in ubuntu user run the following commands<br>
 <code>sudo apt update</code><br>
 <code>sudo apt upgrade</code><br>
 <code>reboot</code><br>

Changing the SSH Port from 22 to 2200<br>
<code>nano /etc/ssh/sshd_config</code><br>
Change line 'Port 22' to 'Port 2200' and save the file

Restart ssh servive<br>
<code>sudo service ssh restart</code><br>
<code>exit</code><br>
 
 New login from local machine<br> 
 <code>ssh -i 'key.pem' ubuntu@13.233.148.2 -p 2200 </code><br>
 
 Configure Timezone to use UTC<br>
 <code>dpkg-reconfigure tzdata</code><br>
  
Creating new user 'grader' and addding it to sudo Group<br>

<code>sudo adduser grader</code><br>
<code>usermod -aG sudo grader</code><br>

Create a file in sudoers.d for user grader<br>
<code>sudo nano /etc/sudoers.d/grader</code><br>
add `grader ALL=(ALL:ALL) ALL` to file and save<br>

Adding SSH Access to the user 'grader'<br>
Login as user grader to the virtual server<br>
<code></code><br>


