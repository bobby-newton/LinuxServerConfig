**LINUX SERVER CONFIGURATION**
*Author: Bobby Newton*
*Completion date: 24/02/2018*

**Project Details:**
Get your server.
1. Start a new Ubuntu Linux server instance on Amazon Lightsail. There are full details on setting up your Lightsail instance on the next page.
2. Follow the instructions provided to SSH into your server.

Secure your server.
3. Update all currently installed packages.
4. Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.
5. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

Warning: When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server. Review this video for details! When you change the SSH port, the Lightsail instance will no longer be accessible through the web app 'Connect using SSH' button. The button assumes the default port is being used. There are instructions on the same page for connecting from your terminal to the instance. Connect using those instructions and then follow the rest of the steps.

Give grader access.
In order for your project to be reviewed, the grader needs to be able to log in to your server.

6. Create a new user account named grader.
7. Give grader the permission to sudo.
8. Create an SSH key pair for grader using the ssh-keygen tool.

Prepare to deploy your project.
9. Configure the local timezone to UTC.
10. Install and configure Apache to serve a Python mod_wsgi application.

If you built your project with Python 3, you will need to install the Python 3 mod_wsgi package on your server: sudo apt-get install libapache2-mod-wsgi-py3.
11. Install and configure PostgreSQL:

Do not allow remote connections
Create a new database user named catalog that has limited permissions to your catalog application database.
12. Install git.

Deploy the Item Catalog project.
13. Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.
14. Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser. Make sure that your .git directory is not publicly accessible via a browser!

**Server Details:**
Public IP: *35.176.149.189*
DNS name of instance: ec2-35-176-149-189.eu-west-2.compute.amazonaws.com

**Account created for user 'grader'**
ssh-key provided in folder with README.md
SSH Login: *ssh -i ~/.ssh/linuxCourse -p 2200 grader@35.178.18.11*
Passphrase for ssh keygen login: *xxxxxxxx*
grader password: *xxxxxxx* (note: password login has been disabled)

**All project actions, updates and changes to server (including security details)**

**Updated all installed packages**
1:  Updated package source list: *sudo apt-get update*
2: Upgraded installed packages: *sudo apt-get upgrade
3: Unattended package updates: *sudo apt-get install unattended-upgrades*

**Auto Removed no longer used packages**
1: *sudo apt-get autoremove*

**Created User Grader, giving sudo access**
1: Created a new user account named grader: *sudo adduser grader*
2: Created the user password: *xxxxxx*
3: Confirmed grader created: *sudo cat /etc/passwd* (displayed the user 'grader')

**Installed Finger**
1: Installed Finger: *sudo apt-get install finger*
2: Tested Finger successful: *finger grader* (displayed grader information)

**Installed Fail2Ban for unsuccessful login attempts**
1: *sudo apt-get update*
2: *sudo apt-get install fail2ban*
3: Service not started, extra update for possible future use. 
	To start use: *sudo service fail2ban start* upon configuring.

**Installed Application status monitoring**
1: Installed Glances: *sudo pip install glances*
2: Tested install by running glances: *glances*

**Sudo Permission granted for 'grader'**
1: Opened root priviledges file: *sudo visudo*
2: Searched for: *root		ALL=(ALL:ALL)	ALL*
3: Under root line added: *grader		ALL=(ALL:ALL)	ALL*
4: Saved and closed the file creating grader with sudo priviledges.

**Created an SSH Key pair for grader using ssh-keygen tool**
1: Used ssh-keygen to create ssh keygen: *ssh-keygen*
2: Created passphrase: *xxxxx*
3 Logged in as 'grader'
4: Created .ssh folder: *mkdir .ssh*
5: Created .ssh/authorized_keys and added information created from local machine: *touch .ssh/authorized_keys*
6: Saved and closed the file

**Set-up permissions on the .ssh file**
1: *sudo chmod 700 ~/.ssh*
2: *sudo chmod 664 ~/.ssh/authorized_keys*

**Checked to see if Password based logins disabled to Force Login using Key Pair**
1: Went to edit sshd_config file: *sudo nano /etc/ssh/sshd_config*
2: Went to the line with Password Authentication. Ensured Yes to No.
3: Saved and closed the file, restarted the ssh service.

**Disabled remote login of root user**
1: Logged in as root, edited the sshd_config file: *sudo nano /etc/ssh/sshd_config*
2: Changed *PermitRootLogin No*
4: Restarted the ssh service.

**Added a further update**
1: *sudo apt-get dist-upgrade
*
**Changed SSH port from 22 to 2200**
1: Entered the sshd_config file for editing: *sudo nano /etc/ssh/sshd_config*
2: Edited the file removing port 22 and replaced with 2200.
3: Saved and closed the file.

**Configured Firewall**
1: Checked UFW status: *sudo ufw status* (displayed as Inactive).
2: Set-up UFW using the below:
3: *sudo ufw default deny incoming*
4: *sudo ufw default allow outgoing*
5: *sudo ufw allow ssh*
6: *sudo ufw allow 2200/tcp (Note: Changed the SSH above to port 2200)*
7: *sudo allow www*
8 *sudo ufw allow 123/udp*
9 *sudo ufw enable*
10: *sudo ufw status (verified ports active and that ufw was active)*

During the Review, requested to disable port 22: *sudo ufw deny 22*

**Configured local Timezone to UTC**
1: Started configuration: *dpkg-reconfigure tzdata*
2: Selected from window: *None of the above, and in second options selected UTC*
3: Confirmed time change using: *date*

**Installed and configured Apache2 **
1: Installed Apache using: *sudo apt-get install apache2*
2: Confirmed successful installation by visiting: *http://35.176.149.189*

**Installed and confirmed to serve a Python mod_wsgi application**
1: Installed python mod_wsgi: *sudo apt-get install libapache2-mod-wsgi*
2: Installed and configured Demo WSGI app: *sudo nano /etc/apache2/sites-enabled/000-default.conf*
3: Added the following at the end of the *<VirtualHost *:80> block before the closing <./VirtualHost>*
	WSGIScriptAlias / /var/www/html/myapp.wsgi
4: Restarted Apache with: *sudo apache2ctl restart*.

**Configured Apache to serve basic WSGI app to confirm installation of Apache and mod_wsgi**
1: Created file: /var/www/html/myapp.wsgi using: *sudo nano /var/www/html/myapp.wsgi
	Within the file placed:
	
def application(environ, start_response):
    status = '200 OK'
    output = 'Hello World!'

    response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
2: Refreshed page to see the above displayed.

**Installed and configured PostgreSQL**
1: Installed PostgreSQL: *sudo apt-get install postgresql postgresql*
2: basic server set-up: *sudo -u postgres psql postgres*
3: Set-up a password for user postgres: *\password postsgres*entered password *xxxxxx*

**Created a new database user with limited permissions to the DB**
1: Connected to the DB as the user postgres: *sudo su - postgres*
2: Generated the PostgreSQL promt using: *psql*
3: Created a new user: *CREATE USER catalog WITH PASSWORD 'xxxxx';*
4: Confirmed user was created and see permissions the user has: *\du*
5: Updated permissions for user 'catalog': 
	*ALTER ROLE catalog WITH LOGIN;*
	*ALTER USER catalog CREATEDB;*
6:  Created DB: *CREATE DATABASE catalog WITH OWNER catalog;*
7: Logged into the DB: *\c catalog*
8: Revoked all rights: *REVOKE ALL ON SCHEMA public FROM public;*
9: Granted only access to the catalog: *GRANT ALL ON SCHEMA public TO catalog;*
10: Exited out of postgresql and postgresql user: *\q then exit*
11: PostgreSQL restarted: *sudo service postgresql restart*

**Git Installed**
1: Installed Git: *sudo apt-get install git*
2: Edited the Git Configuration:
	*git config --global user.name "My Name"*
	*git config --global user.email myemail@somedomain.com*
3: Confirmed: *git config --list*

**Cloned Item Catalogue App to the server**
1: Went into: *cd /var/www*
2: Created dir catalog: *sudo mkdir catalog*
3: Went into: *cd catalog*
4: Cloned repo for item catalogue: *https://github.com/bobby-newton/ItemCatalog*
	Project location is now: */var/www/catalog/catalog*

**Ensured the .git directory is not publicly accessible via a browser **
1: Created a .htaccess file in the .git folder and placed the lines:
	*Order allow,deny*
	*Deny from all*
	*RedirectMatch 404 /\.git*

**Installed Flask and created the Virtual Environment  for the Item Catalogue**
1: Ensured was in: *cd /var/www/catalog/catalog*
2: *sudo apt-get install python-pip*
3: *sudo pip install virtualenv*
4: Gave command for virtual environment: *sudo virtualenv venv*
5: Activated the virtual environment: *source venv/bin/activate*
6: Installed dependencies for Database: 
	*sudo pip install Flask*
	*sudo pip install sqlalchemy*
	*sudo pip install Flask-SQLAlchemy*
	*sudo pip install oauth2client*
	*sudo pip install httplib2*
	*sudo pip install requests*
	*pip install werkzeug*
	*pip install Flask-Login*
More was added and upgraded. To see package list use: *pip freeze*
7: Configured Apache Virtual Host: *sudo nano /etc/apache2/sites-available/catalog.conf*
8: Enabled the Virtual Host: *sudo a2ensite catalog*
9: Restarted the service: *service apache2 restart*
10: WSGI file configured:
	*cd /var/www/catalog or cd ..*
	*sudo nano catalog.wsgi*
	*Added following to the catalog.wsgi:*
		*#!/usr/bin/python 
		import sys 
		import logging 
		logging.basicConfig(stream=sys.stderr) 
		sys.path.insert(0,"/var/www/catalog/")  
		from catalog import app as application 
		application.secret_key = 'awesome_secret_key'*
	*Saved and exited the file*
11: Apache restarted: *sudo service apache2 restart*
12: Modified database calls in Item Catalog. Switching from SQLite to PostgreSQL.
	Edited the files: *app.py, db_table_seed.py and db_data_seed.py with the following:*
	*Replaced: engine = create_engine('sqlite:///cataloguelist.db)* with
	*engine = create_engine('postgresql://catalog:password@localhost/catalog')*
13: Modifed app.py to display the full path to clients_secrets.jason.
14: Renamed app.py to __init__.py

**Displayed the Item Catalogue Site**
1: Added EC2 URL to the VirtualHost:
	*sudo nano /etc/apache2/sites-enabled/catalog.conf*
	Added: *ServerAlias ec2-35-176-149-189.eu-west-2.compute.amazonaws.com*
	Saved and closed the file.
2: Added EC2 and Public IP to the hosts file:
	*sudo nano /etc/hosts*
	Added the hosts.
	Saved and closed the file.
3: Checked what sites are enabled: *ls -alh /etc/apache2/sites-enabled/*
4: Restarted Apache2: *sudo service apache2 restart*

**Updated OAuth Information for Google Login**
1: Went to google developers console.
2: Went to credentials and selected Item Catalogue Project.
3: Added the following URLS to Authorized Javascript origins amd Authorized redirect, both local URL and EC2:
	*http://35.176.149.189*
	*http://ec2-35-176-149-189.eu-west-2.compute.amazonaws.com*
4: Restarted Apache2: *sudo service apache2 restart*

**Started Item Catalogue Application**
1: *cd /var/www/catalog/catalog*
2: *sudo python __init__.py*

**Tested Configuration**

**User Management**
1: grader has the ability to log in with the generated ssh-key.
2: Remote login of the root user disabled.
3: grader has access and can run sudo commands. 

**Security**
1: Firewall configured to allow only SSH, HTTP and NTP.
2: Key-based SSH authentication is enforced.
3: All system packages have been updated to most recent versions.
4: SSH is hosted on non-default port.

**Application Functionality**
1: The web server responds on port 80.
2: Database server has been configured to serve data using PostgreSQL.
3: Web server has been configured to serve the Item Catalog application as a WSGI app.

**Documentation**
1: A README file is included in the GitHub repo containing the following information: IP address, URL, summary of software installed and summary of configurations made.


