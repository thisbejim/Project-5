# Project 5 

## Test

The hosted catalog project is [here](http://54.191.27.210/)

## Process

I used tips from the following pages to help complete this project.

Package updates:
http://askubuntu.com/questions/196768/how-to-install-updates-via-command-line
http://stackoverflow.com/questions/28253681/you-need-to-install-postgresql-server-dev-x-y-for-building-a-server-side-extensi

File editing:
http://stackoverflow.com/questions/17535428/how-to-edit-save-a-file-through-ubuntu-terminal

Ports and Firewall:
http://ubuntuforums.org/showthread.php?t=1096398
http://ubuntuforums.org/showthread.php?t=1739013
https://help.ubuntu.com/community/UFW
http://www.howtogeek.com/115116/how-to-configure-ubuntus-built-in-firewall/

Timezone:
http://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt

Host issues:
https://forums.aws.amazon.com/message.jspa?messageID=495274

Postgres and Apache:
https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps
http://blog.udacity.com/2015/03/step-by-step-guide-install-lamp-linux-apache-mysql-python-ubuntu.html
http://stackoverflow.com/questions/13733719/which-version-of-postgresql-am-i-running

Flask:
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

Users:
http://askubuntu.com/questions/168280/how-do-i-grant-sudo-privileges-to-an-existing-user
http://askubuntu.com/questions/410244/a-command-to-list-all-users-and-how-to-add-delete-modify-users
http://askubuntu.com/questions/136788/how-do-i-list-the-members-of-a-group
http://stackoverflow.com/questions/11919391/postgresql-error-fatal-role-username-does-not-exist

The most challenging part was creating a new postgres user/password to use to access the catalog app database, simply because I found the default postgress user/role confusing at first. Setting up the flask app in general was a challenge and because I used virtualenv the .wsgi could not find my app's supporting libraries. Someone had a great fix for this:

activate_this = '/var/www/mentor/venv/bin/activate_this.py'

execfile(activate_this, dict(__file__=activate_this))

I was very grateful to find this little snippet as this particular problem was really stalling my progress.

The catalog app had previously been setup to work with sqlite so I had to change the engine creation to use postgresql and access the app database with a new postgres role I had created earlier. 

While most everything for the app had been installed correctly using pip install -r requirements.txt in virtualenv, I kept getting a missing package error for psycopg2. I tried to pip install it directly, but in the end I had to use sudo apt-get install python-psycopg2 to get psycopg2 to install properly.

I got stumped trying to edit my postgres pg_hba.conf file because I had been using sudo nano /etc/postgresql/9.1/main/pg_hba.conf when my postgres version was actually 9.3 not 9.1. psql --version gave me this answer.





