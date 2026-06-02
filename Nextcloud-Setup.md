Installation Guide for Nextcloud with Apache, PHP, and MySQL
Follow this guide to install Nextcloud with Apache, PHP, and MySQL on Ubuntu 26.02


1. Update the system

``bash
sudo apt update && sudo apt upgrade -y``

2. Install Apache, MariaDB, and PHP

``bash
sudo apt install apache2 mariadb-server libapache2-mod-php php php-mysql php-xml php-mbstring php-curl php-zip php-gd php-intl php-bcmath unzip -y``

3. Download Nextcloud

``bash
cd /tmp 
wget https://download.nextcloud.com/server/releases/latest.zip``

4. Install and extract the ZIP file

``bash
sudo apt install unzip
unzip latest.zip``

5. Move Nextcloud to the web directory

``bash
sudo mv nextcloud /var/www/``

6. Set permissions for Nextcloud

``bash
sudo chown -R www-data:www-data /var/www/nextcloud 
sudo chmod -R 755 /var/www/nextcloud``

7. Return to home directory

``bash
cd ..``

8. Create Apache configuration for Nextcloud

``bash
sudo nano /etc/apache2/sites-available/nextcloud.conf``

9. Add VirtualHost configuration

``<VirtualHost *:80>
    ServerName your-domain.com
    DocumentRoot /var/www/nextcloud
    <Directory /var/www/nextcloud/>
        Require all granted
        AllowOverride All
        Options FollowSymLinks MultiViews
    </Directory>
</VirtualHost>``


10. Enable the Nextcloud site and Apache modules

``bash
sudo a2dissite 000-default.conf 
sudo a2ensite nextcloud.conf 
sudo a2enmod rewrite headers env dir mime``

11. Reload and restart Apache

``bash
sudo systemctl reload apache2 
sudo systemctl restart apache2``

12. Log into MariaDB

``bash
sudo mysql -u root -p``

13. Create Nextcloud database and user

``sql
CREATE DATABASE nextcloud;
CREATE USER 'insert_database_username'@'localhost' IDENTIFIED BY 'insert_very_complex_password';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'insert_database_username'@'localhost';
FLUSH PRIVILEGES;``

14. Exit MariaDB

``sql
exit``

15. Enable Apache and MariaDB on startup

``bash
sudo systemctl enable --now apache2
sudo systemctl enable --now mariadb``

16. Verify that Apache is running

``bash
sudo systemctl status apache2 --no-pager``

17. Verify that MariaDB is running

``bash
sudo systemctl status mariadb --no-pager``

You can now access Nextcloud in your preferred web browser.
