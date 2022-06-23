# Configuring Wordpress for LAMP stack.

Prerequisites
1) Access to an ubuntu server with a sudo user
2) Install LAMP stack 
3) Secure your site with SSL.

### Step 1 - Creating a MySQL Database and user for WordPress.
`sudo mysql`
`mysql -u root -p`

Now we'll create a new database that wordpress will control.
`mysql> CRATE DTABASE wordpress DEFAULT CHARACTER SET utf8_unicode_ci;`
`GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'password';`

After creating this user, flush the privileges to ensure that the current instance of MySQL knows about the recent changes you’ve made
`EXIT;`
You now have a database and user account in MySQL, each made specifically for WordPress.

### Step 2 - Installing Additional PHP Extensions
First, update your package list:
`sudo apt update`

Next download and install some of the most poplular PHP extensions for use with wordpress.
`sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip`

`sudo systemctl restart apache2`

### Step 3 - Adjusting Apache's Configuration to Allow for .htaccess Overrides and Rewrites 

To enable, open the virtual host file for your website:
`sudo nano /etc/apache2/sites-available/wordpress.conf`

Once you’ve opened this file, you’ll notice that the use of .htaccess files is disabled. To allow .htaccess files, you need to set the AllowOverride directive within a Directory block pointing to your document root. Add the following block of text inside the VirtualHost block in your configuration file. Make sure that you use your own web root directory in place of the highlighted example:


`<Directory /var/www/wordpress/>
	   AllowOverride All
</Directory>`
When you are finished, save and close the file. If you’re using nano, you can exit by pressing CTRL + X then Y and ENTER.

Next, enable mod_rewrite so that you can utilize the WordPress permalink feature:
`sudo a2enmod rewrite`
`sudo apache2ctl configtest`

Restart Apache to implement the changes:
`sudo systemctl restart apache2`

### Step 4 - Downloading Wordpress
Now that your server software is configured, you can download and set up WordPress. For security reasons, it is always recommended to get the latest version of WordPress from their site.

First change into a writable directory:
cd /tmp
Then download the compressed release by runnung the following: 
curl -o curl -O https://wordpress.org/latest.tar.gz
Extract the compressed file to create the wordpress directory structure:
tar xzvf latest.tar.gz

You’ll be moving these files into your document root momentarily. Before you do, add an empty .htaccess file so that this will be available for WordPress to use later.

Create the file by running the following:
`touch /tmp/wordpress/.htaccess`
Then make a copy of the sample configuration file and name it wp-config.php, the filename that Wordpress actually reads:
Then, make a copy of the sample configuration file and name it `wp-config.php` , the filename that Wordpress actually reads:
`cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php` 

Lastly, create the upgrade directory, so that wordpress won't run into permissions issues when trying to do this on its own following an update to software:
`mkdir /tmp/wordpress/wp-content/upgrade`

Now you can copy the entire contents of the directory into your document root. Using a dot at the end of your source directory indicates that everything within the directory should be copied, including hidden files (like the .htaccess file you created). Again, use the name of your actual document root in place of the highlighted example:

`sudo cp -a /tmp/wordpress/. /var/www/wordpress`

### Step 5 - Configuring the Wordpress Directory (**Important**)
Before you begin the web-based WordPress setup, you need to adjust some items in your WordPress directory.

**Adjusting the Ownership and Permissions**
One of the big things you need to accomplish is setting up reasonable file permissions and ownership.

Start by giving ownership of all the files to the www-data user and group. This is the user that the Apache web server runs as, and Apache will need to be able to read and write WordPress files in order to serve the website and perform automatic updates.

Update the ownership with chown:
`sudo chown -R www-data:www-data /var/www/wordpress`
Next run two find commands to set the correct permissions on the WordPress directories and files:
`sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;`

**Setting up the WordPress Configuration File**

Now, you need to make some changes to the main WordPress configuration file.

When you open the file, your first order of business will be to adjust some secret keys to provide some security for installation. WordPress provides a secure generator for these values so that you do not have to try to come up with good values on your own. These are only used internally, so it won’t hurt usability to have complex, secure values here.

To grab secure values from the WordPress secret key generator, run the following:
curl -s https://api.wordpress.org/secret-key/1.1/salt/

Now, open the WordPress configuration file. Make sure the file path aligns with your own document root information as highlighted in the following:
`sudo nano /var/www/wordpress/wp-config.php`
This setting can be added after the database connection settings, or anywhere else in the file:
`. . .

define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpressuser');

/** MySQL database password */
define('DB_PASSWORD', 'password');

. . .

define('FS_METHOD', 'direct');`
Save and close the file when you are finished.

### Step 6 – Completing the Installation Through the Web Interface
Now that the server configuration is complete, you can complete the installation through the web interface.

In your web browser, navigate to your server’s domain name or public IP address:
`https://server_domain_or_IP`
