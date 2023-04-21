# Distributed MVC Architecture

Distributed MVC (Model-View-Controller) architecture is a software design pattern that divides an application into three interconnected components: the Model, the View, and the Controller. The architecture is designed to be distributed across multiple servers, allowing for improved scalability and fault tolerance.

## Task

  my objective is to build such architecture using VirtualBox by creating several virtual machines; one for the [HTTP server](https://fr.wikipedia.org/wiki/Serveur_web) and another for the database server.
## Architecture

  ![MVC Architecture](images/mvc-architecture.png)
## Requirement 

* I assume you have VirtualBox Installed and you are comfortable creating virtual machines. *Note* that in this repos we are going to work with ubuntu server machines ([link to download ISO image](https://ubuntu.com/download/server)) not ubuntu Desktop.
* the host OS doesn't matter in our case. I have Windows as host OS, Linux and MacOs are fine too.
## Network Configuration

1. create 2 Ubuntu server vms one for the **Apache** server, the other for the **MySql** server, you are free to give the hardware specs (e.g `20Go storage 2Go ram, ..`)
2. create an internal network between the Apache server and MySql server.
> set this network in adapter 1 of the apache server
3. create a bridged network between the host and the apache server.
> set this in adapter 2 of the apache server
4. use `ifconfig` (or alternatively`ifconfig -a`) command to view network interface configuration information. 
5. in case where a network interface is missing an ip address, use the **dhcp** protocol to generate an ip address.
> use `sudo dhcp <network-Interface-Name>`

## Apache Sever Configuration

1. install apache2 in your vm.
> sudo apt update \
  sudo apt install apache2

to make sure apache is installed run `sudo systemctl start apache2`, enter password when prompted, then tested it out by typing in your ip address.

2. configure the page to be loaded 

    1. create a new directory inside `/var/www/` (name it *gci*)
    2.  add a configuration file inside `/etc/apache2/sites-available` (name it *gci.conf*)
    3. copy the content of the default configuration to our gci configuration file using`sudo cp 000-default.conf gci.conf`
    4. create an `index.html` file inside `/var/www/` and add some content.
    5. change to `/etc/apache2/sites-available` using the `cd` and run the following commands:
    > `a2dissite 000-default.conf`\
      `a2ensite gci.cong`\
      `sudo systemctl reload apache2`

Now you are able to connect to the apache server from your host OS; just tap the ip address of the apache server in your browser. 

## MySql server configuration
At this stage, we will connect the apache server with the database server

1. first run the command `sudo apt-get install mysql-client` in your apache server
2. now go to the mysql server, and make sure that mysql is installed (if not run `sudo apt install mysql`)
3. log in to mysql cli as a root (`sudo mysql`)
4. now, create a new mysql user, this user has to be accessed from the apache server in order to create the connection. \
use the following command to achieve that :
> CREATE USER "username"@"ip-address-of-apache-server" identified by "password";

5. now grant this new user all the privileges using the following command :
> GRANT ALL PRIVILEGES  ON * . * TO "user-created"@"ip-address-of-apache";
 
7. go back to your apache server and test your connection (use `mysql -u <userName> -h <mysql-ip-address> -p` enter password when prompted)
8. On apache server , open `/etc/apache2/apache2.conf` file whith your favorite text editor .\
add the following text and replace with your corresponding information :
```
<IfModule mod_dbd.c>
  		DBDriver mysql
  		DBDParams "host=<mysql_vm_ip_address> port=3306 dbname=<database_name> user=<username> pass=<password>"
/IfModule>
```
then save and close the file

9. restart apache server using `sudo systemctl restart apache2`

Now your apache server can connect with mysql. You can test it with a php script for example.