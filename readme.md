# Linux Server Configuration

### Overview:
Use a baseline installation of a Linux distribution on a virtual machine (AWS server) and prepare it to:
	Install updates
	Secure the server from a number of attack vectors 
	Installing/configuring web and database servers
	Host your web applications
	
## 1.Launch your Virtual Machine with your Udacity account and log in.
mv ~/Downloads/udacity_key.rsa ~/.ssh/
chmod 600 ~/.ssh/udacity_key.rsa
ssh -i ~/.ssh/udacity_key.rsa root@52.43.68.7


## 2. Create a new user named grader and grant this user sudo permissions.
adduser grader
visudo
add:  
grader  ALL=(ALL:ALL) ALL
cut -d: -f1 /etc/passwd

## 3. Update all currently installed packages.
sudo apt-get update
sudo sudo apt-get upgrade
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades


## 4. Configure the local timezone to UTC.
sudo dpkg-reconfigure tzdata
sudo apt-get install ntp
sudo shutdown -r now

## 5. Change the SSH port from 22 to 2200
vim /etc/ssh/sshd_config
/etc/init.d/ssh restart
ssh-keygen
ssh-copy-id grader@52.43.68.7 -p2200
ssh -v grader@52.43.68.7 -p2200
sudo vim /etc/ssh/sshd_config

## 6. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
sudo ufw enable
sudo ufw status verbose
sudo ufw allow 80/tcp
sudo ufw allow 123/udp



## 7. Install and configure Apache to serve a Python mod_wsgi application
sudo apt-get install apache2
http://52.43.68.7/
sudo apt-get install python-setuptools libapache2-mod-wsgi
sudo service apache2 restart
echo "ServerName HOSTNAME" | sudo tee /etc/apache2/conf-available/fqdn.conf
sudo a2enconf fqdn


## 8. Install and configure PostgreSQL:
sudo apt-get install postgresql postgresql-contrib
sudo vim /etc/postgresql/9.3/main/pg_hba.conf
sudo vim database_setup.py
sudo vim application.py
mv application.py __init__.py
sudo adduser catalog
exit
exit 

## 8b. Create a new user named catalog that has limited permissions to your catalog application database.
ssh -v grader@52.43.68.7 -p2200
sudo -u postgres -i
psql
CREATE USER catalog WITH PASSWORD 'PASSWORD_FOR_CATALOG';
ALTER USER catalog CREATEDB;
\du
CREATE DATABASE catalog WITH OWNER catalog;
\c catalog
REVOKE ALL ON SCHEMA public FROM public;
GRANT ALL ON SCHEMA public TO catalog;
\q
exit
pwd
cd /var/www/catalog/catalog/
sudo service apache2 restart
cd /var/www/catalog/catalog/
http://www.hcidata.info/host2ip.cgi
use ip address (52.43.68.7) to get host name: ec2-52-43-68-7.us-west-2.compute.amazonaws.com
sudo vim /etc/apache2/sites-available/catalog.conf
sudo a2ensite catalog


## 9. Install git, clone and set up your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!
sudo apt-get install git
git config --global user.name "..."
git config --global user.email "...@gmail.com"
sudo apt-get install libapache2-mod-wsgi python-dev
sudo a2enmod wsgi
cd /var/www
sudo mkdir catalog
cd catalog
sudo mkdir catalog
cd catalog
sudo mkdir static templates
sudo nano __init__.py
sudo apt-get install python-pip
sudo pip install virtualenv
sudo virtualenv venv
sudo chmod -R 777 venv
source venv/bin/activate
pip install Flask
python __init__.py
http://52.43.68.7:5000/
deactivate
sudo nano /etc/apache2/sites-available/catalog.conf
sudo a2ensite catalog
cd /var/www/catalog
sudo vim catalog.wsgi
sudo service apache2 restart
git clone https://github.com/.../FSND_P3_Item_Catalog.git
cd FSND_P3_Item_Catalog
mv *.* /var/www/catalog/catalog/
cd /var/www/catalog/catalog/
ls -l
mv *.css static/
mv *.html templates/
cd /var/www/catalog/
rmdir FSND_P3_Item_Catalog
sudo vim .htaccess
cd /var/www/catalog/catalog/
source venv/bin/activate
pip install httplib2
sudo pip install flask-seasurf
sudo pip install --upgrade oauth2client
sudo pip install sqlalchemy
sudo apt-get install python-psycopg2

## Resources Used:
https://udacity.com/
https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2
http://askubuntu.com/questions/59458/error-message-when-i-run-sudo-unable-to-resolve-host-none
https://ubuntuforums.org/showthread.php?t=2169570
http://stackoverflow.com/questions/27211158/no-passwd-entry-for-user-postgres-error
http://stackoverflow.com/questions/31168606/internal-server-error-target-wsgi-script-cannot-be-loaded-as-python-module-and
https://developers.google.com/+/web/signin/#set_up_google_sign-in_for_google
https://developers.google.com/api-client-library/python/guide/aaa_oauth#OAuth2WebServerFlow



