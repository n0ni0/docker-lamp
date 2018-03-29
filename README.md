n0ni0/lamp
===========

This docker-compose create a lamp environment ready to develop with the feature to have all your proyects reusing the same containers.
You don´t need to create new containers when start a new proyect and save space in your disk without repeated images with minimal changes.

Includes the following:

* Apache
* Mysql 5.6
* PHP-FPM 7.1.* with the following extensions and programs:
* Opcache
* XDebug
* gd (with freetype and jpeg support)
* iconv
* mcrypt
* curl
* dom
* hash
* pdo
* pdo_mysql
* simplexml
* soap
* sudo
* git
* cron
* wget
* libfreetype6-dev
* libjpeg62-turbo-dev
* libmcrypt-dev
* libpng12-dev
* libxml2-dev
* libcurl4-openssl-dev
* libxslt-dev
* libicu-dev
* nano
* vim
* nodejs
* grunt-cli
* composer
* PHPMyAdmin
* MailCatcher

Services:
----
* db: MySql server (listening on port 3306)
* fpm: PHP-FPM listening on port 9000.
* mailcatcher: (accessible on port 1080 with your web brower) 
* phpmyadmin: (accessible on port 8080 with your web brower)
* apache: listening on http (80) only

Folder structure
----
* www : Put your proyect folder there.
* db/init : Any scripts (SQL or sh) placed there will be ran on the first machine build. Usefull for importing a database
* db/data : This is where the MySQL database are stored. You can copy your MySQL database straight there too (it might require some tweaks though, have a read at the MySQL docker image documentation).

Database configuration
----
The MySQL database configuration are passed to the MySQL instance as environment variables set inside the docker-compose.yml file. They are used to initialize the MySQL instance on the first run. Feel free to adjust them as you need.

Configuration customization
----
* Apache default site configuration is exposed in 'apache/hosts/default.conf'. If you want to add new hosts, do it in 'apache/hosts/' and reload the apache service, remember to add to your 'hosts' file like '127.0.0.1 proyect.devel.com'.
* PHP configuration can be tailored by editing the 'fpm/conf/custom.ini'.

Instructions to use it
----
Install with the command:
```
docker-compose up -d
```
Create a new virtualhost in apache/hosts using as basic reference default.conf

Create a new host in your computer:
```
vim /etc/hosts
```
See the name of all containers:
```
docker ps -a
```
See the name of running containers:
```
docker ps
```
Restart apache container:
```
docker restart lamp_apache_1
```
Enter in a bash of a container:
```
sudo docker exec -it name bash
```
Restart a container:
```
sudo docker restart name
```
To stop all containers:
```
sudo docker stop $(sudo docker ps -a -q)
```

PHP Debugging
----
By default the remote port is set to 9009 and the remote handler to dbgp. The connect_back option is enable, so ensure your IDE is allowing it.

Adding further features
----
More PHP extensions can be added by customizing the dockerfile inside the FPM folder. Simply rebuild the FPM image after by running docker-compose up --build.
If it fail, stop the fpm container, remove it, remove the image and execute again 'sudo docker-compose up -d'.