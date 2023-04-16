# Distributed MVC Architecture

Distributed MVC (Model-View-Controller) architecture is a software design pattern that divides an application into three interconnected components: the Model, the View, and the Controller. The architecture is designed to be distributed across multiple servers, allowing for improved scalability and fault tolerance.

## Task

  my objective is to build such architecture using VirtualBox by creating several virtual machines; one for the [HTTP server](https://fr.wikipedia.org/wiki/Serveur_web) and another for the database server.
## Architecture

  ![MVC Architecture](images/mvc-architecture.png)
## Requirement 

* I assume you have VirtualBox Installed and you are comfortable creating virtual machines note in this repos we are going to work with ubuntu server machines ([link to download ISO image](https://ubuntu.com/download/server)) not ubuntu Desktop.
* the host OS doesn't matter in our case. I have Windows as host OS, Linux and MacOs are fine too.
## Configuration

1. create 2 Ubuntu server vms one for the **Apache** server, the other for the **MySql** server, you are free to give the hardware specs (e.g `20Go storage 2Go ram, ..`)
2. create an internal network between the Apache server and MySql server.
> set this network in adapter 1 of the apache server
3. create a bridged network between the host and the apache server.
> set this in adapter 2 of the apache server
4. use `ifconfig` (or alternatively`ifconfig -a`) command to view network interface configuration information. 
5. in case where a network interface is missing an ip address, use the **dhcp** protocol to generate an ip address.
> use `sudo dhcp <network-Interface-Name>`