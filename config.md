#Config

##Login
ssh -i ~/.ssh/id_rsa -p 2200 grader@54.149.31.80

##Automatic Updates
unattended-upgrades package was already installed on the system, ran sudo dpkg-reconfigure -plow unattended-upgrades to enable it. 

##Changing SSH port
I used sudo nano /etc/ssh/sshd_config to open and edit the SSH settings, and change the port to 2200.

##Firewall
First of all I enabled the firewall with sudo ufw enable. Then I used sudo ufw status to see what was going on with the firewall by default. I don’t quite remember but I do think there were just zero rules in there to start. So I used sudo ufw allow 22/tcp (later I deleted this rule while logged in on port 2200), sudo ufw allow ssh, sudo ufw allow 80/tcp, sudo ufw allow ntp to set the firewall rules.

##Timezone
sudo dpkg-reconfigure tzdata to access time selection and changed it to UTC.

##Update
Used sudo apt-get upgrade and sudo apt-get dist-upgrade

##Add user
sudo adduser grader

##Apache
Used sudo apt-get install apache2, sudo apt-get install python-setuptools libapache2-mod-wsgi to install dependences. 

##Postgres
Install: sudo apt-get install postgresql
Change to default psql user: sudo -i -u postgres
Access database: psql 
Create user: create user mentor -–no-superuser 
Change password: alter user mentor with password ‘thismajorpass’
Also had to exit out to root user: \q followed by exit
To create the mentor user: adduser mentor
Later I use this user to connect my application to the database with postgresql://mentor:thismajorpass@localhost:5432/mentor
Login to new user: su – mentor, psql
Create database – create database mentor;
Exit: /q, exit
Log back into postgres default user: su – postgres, psql
\connect mentor
Change the catalog user privleges: GRANT CONNECT ON DATABASE mentor TO catalog; GRANT SELECT ON ALL TABLES IN SCHEMA public TO catalog;
Not allowing remote connections is setup by default.

##Git
sudo apt-get install git

##Flask + wsgi
Install: sudo apt-get install libapache2-mod-wsgi python-dev
Enable mod_wsgi: sudo a2enmod wsgi
Create directory in /var/www: git clone my app repository
Install pip: sudo apt-get install python-pip
Install virtual env: sudo pip install virtualenv
Create environment: sudo virtualenv venv
Activate it: source venv/bin/activate
Install all app dependencies with the requirements.txt file from my project: pip install –r requirements.txt
Deactivate environment: deactivate
Configure and enable virtual host:
Sudo nano /etc/apache2/site-available/mentor.conf and edit to configure
Enable it: sudo a2ensite mentor
Create the .wsgi file: cd /var/www/mentor, sudo nano mentor.wsgi
Added this:
!/usr/bin/python
activate_this = '/var/www/mentor/mentor/venv/bin/activate_this.py'
execfile(activate_this, dict(__file__=activate_this))
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/mentor/")
from project import app as application
application.secret_key = 'Add your secret key’
Restart apache: sudo service apache2 restart

##Reboot server
sudo shutdown -r now
