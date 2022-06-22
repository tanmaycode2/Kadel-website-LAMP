
# Kadel Labs -project

### Deploying website on LAMP stack on EC2 server.

**Step1 - Lauch Ec2 instance of linux(ubuntu)**

**Step 2 - Install apache on it.**

`sudo apt install apache2`

Adjust the Firewall to Allow Web Traffic.

`sudo ufw app list`

 If you look at the Apache Full profile details, you’ll see that it enables traffic to ports 80 and 443:
	
`sudo ufw app info "Apache Full"`

`sudo ufw allow "Apache Full"`

Get your servers Public Ip Address.

`sudo apt install curl`
`curl http://icanhazip.com`


**Step 3 - Install Mysql on the Ec2 server**

`sudo apt install mysql-server`

This command, too, will show you a list of the packages that will be installed, along with the amount of disk space they’ll take up. Enter Y to continue

Now run a simple security script that comes pre-installed with MySQL which will remove some dangerous defaults and lock down access to your database system. Start the interactive script by running:

`sudo mysql_secure_installation`

`sudo mysql`

Output
====================================================================
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.34-0ubuntu0.18.04.1 (Ubuntu)`
`
Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

=====================================================================

`mysql> exit`


**Step 4 - Finally Install PHP on it.**

`sudo apt install php libapache2-mod-php php-mysql`

**Step 5 - Clone the Git repository in **
`/var/www/html folder

And now just go to your EC2 public Ip Address 
For eg : http://43.204.224.0/

And your will see something your website deployed on ec2 instance.


![Kadel site deployed on EC2(LAMP stack)](https://user-images.githubusercontent.com/96629547/175033894-bac95aa4-4ed6-41ce-89d7-5ec4006d741a.jpg)

![website deployed lamp stack](https://user-images.githubusercontent.com/96629547/175033942-ce008b63-ca61-4e37-9608-6c7af1a39946.jpg)
