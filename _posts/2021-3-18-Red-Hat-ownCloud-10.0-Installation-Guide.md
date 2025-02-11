# Overview of ownCloud Server
Using ownCloud, you can store your data in a central repository either on an on-premise data center or at our hosted data center. As an administrator, you can control the access and the extent of use of the data that is stored in your ownCloud server.

You can perform the following tasks to enhance the security of the data:

* Enable multi-factor authentication for secure access to data.
* Integrate and authenticate users against the identity providers you use.
* Control users and their level of access.
* Automate data usage policies that reduce the frequency of the need for your intervention.
* Create and add custom usage policies for the data to control its use downstream.
* Create policies to control the extent and use of data sharing outside the private network.
 
# Understanding the ownCloud Server Package

The ownCloud package comprises the server and the client applications for both desktop and mobile users. Depending on your organization’s subscription mode, the server applications that are packaged with the core server vary.

The ownCloud package comprises the following components:

* ownCloud server – Runs on Linux environment. If your organization is on Enterprise Subscription mode, your server package includes the Enterprise apps.
* ownCloud client for desktop users – Runs on Linux, Microsoft Windows, and Apple Mac OS X.
* ownCloud client for mobile users – Runs on Android and Apple iOS.

# System Requirements
This section lists the requirements for installing ownCloud. The versions listed are suppported and certified. 
## Server  Requirements
**Operating System**
* Ubuntu 16.04 and 18.04
* Debian 7, 8, and 9
* SUSE Linux Enterprise Server 12 with SP1, SP2, and SP3
* Red Hat Enterprise Linux/Centos 6.9, 7.3, 7.4, and 7.5
* Fedora 27, 28, and 29
* openSUSE Leap 42.3 and 15

**Database**
* MySQL or MariaDB 5.5+
* Oracle 11g (Supported only for Enterprise editions)
* PostgreSQL 9 (versions 10 and above are not yet supported)
* SQLite (Not suitable for production environment)

**Web server** 
* Apache 2.4 with prefork and mod_php

**PHP Runtime**
5.6, 7.0, 7.1, and 7.2

**NOTE**: PHP versions earlier than 7.2 could be deprecated in future.
 
**Hypervisors**
* Hyper-V 
* VMWare ESX
* Xen
* KVM
 

## Client Requirements 
The oldest client versions you can use to connect to the ownCloud server are as follows:
* Desktop Client 2.3.3
* Android App
* iOS App   

**Desktop**
* Windows 7+
* Mac OS X 10.7+ (64-bit only)
* CentOS 6 & 7 (64-bit only)
* Debian 8.0 & 9.0
* Fedora 27 & 28
* Ubuntu 16.04 & 18.04
* openSUSE Leap 42.2 & 42.3

**Mobile**
* iOS 9.0+
* Android 4.0+

**Web Browser**

* Edge (current version on Windows 10)
* IE11+ (except Compatibility Mode)
* Firefox 57+ or 52 ESR
* Chrome 66+
* Safari 10+
    
# Installing the ownCloud Server

ownCloud offers different methods of installation to suit different requirements. Based on your preference, you can choose to install ownCloud server in any one of the following methods:

* Manual installation – Use when you prefer to install and configure individual components and packages separately. For more information, see XXX.
* Docker installation –  Use when you must expose port 8080 and authorize HTTP connections. For more information, see Introduction to Docker-Based Installation.
* Command Line installation – Use if you prefer a non-GUI based installation or if your environment depends on scripted operations. For more information, see XXX.
* Wizard installation – Use only when you can complete the installation procedure with access control mechanisms. If you install using the wizard on public internet, you risk the presence of an external threat. For more information, see XXX.
* Linux Package Manager installation – Use only for single-server setups and to avoid any automatic package upgrades. For more information, see XXX.
## Introduction to Docker-Based Installation

If you are in a Dockerized environment, you can use the ownCloud server docker image to perform the installation. You can use the Docker image for evaluation purposes and for installation in a Docker-compose setup in a production environment.

Be aware of the following features of Docker Compose-based installation:
* Exposes 8080 port and allows HTTP connections.
* Uses separate MariaDB and Redis containers.
* Mounts the data and MySQL data directories on the host for persistent storage.
* Stores all files in Docker volumes and not as a physical filesystem tree.

## Installing ownCloud Using Docker

Follow this procedure as is for local installations.  For remote installations, make the necessary customizations.

1. Get a copy of the ownCloud server stack from: [https://hub.docker.com/r/owncloud/server/tags](https://hub.docker.com/r/owncloud/server/tags).

2. Create a new directory by running the following command:
`sudo mkdir <owncloud docker server>`
3. Go to the newly created directory by running the following command:
`cd <owncloud docker server>`
4. Copy the sample  docker compose .yml file from the GitHub repository by running the following command:
`wget https://raw.githubusercontent.com/owncloud/docs/master/modules/admin_manual/examples/installation/docker/docker-compose.yml`
5. Create the environment configuration file with your environment values.
Run the following command:

`cat << EOF > .env`\ <br>
`OWNCLOUD_VERSION=<ownCloud version number to install>`<br>
`OWNCLOUD_DOMAIN=localhost:8080`<br>
`ADMIN_USERNAME=<username for admin user>`<br>
`ADMIN_PASSWORD=<password for the admin username>`<br>
`HTTP_PORT=8080`<br>
`EOF`<br>

**NOTE**: Once set, the username is not editable.

**NOTE**: If you are installing on a remote system, replace the OWNCLOUD_DOMAIN values with the appropriate value.

6. Build and start the containers by running the following command:
`sudo docker-compose up -d`
7. Verify that the containers have started by running the following command:
`sudo docker-compose ps`
 The output displays the status **Up** if the containers are up and running.

## Performing Post-Installation Tasks

After the installation is complete, since the files are stored as Docker volumes, you must persist the files manually.

1. Inspect the volumes by running the following command: `sudo docker volume ls | grep ownclouddockerserver`
    
1. Export the files as a tar archive file by running the following command:`sudo docker ` `run -v ownclouddockerserver_files:/mnt` `
 ubuntu tar cf - -C /mnt . > files.tar`

1. After the containers are up, it will take some time for the services to become fully functional. You can view the log of the services of the applications defined by Docker Compose by running the following command: `sudo docker-compose logs --follow owncloud`

1. Start the web UI to verify the installation after you see the following message in the log file: `Starting apache daemon`

## Verifying the Installation

To verify a successful installation, you must access the ownCloud server from a browser. 

1. On the address bar of a supported browser, enter the following address:
`http://localhost:8080`
1. Enter the username and the password you set in the .env file at the time of installation, and click enter.

For Troubleshooting Tips, see XXX. 
# Adding a New User

After you complete the installation, you can begin adding users who can use your ownCloud server space. You can add a new user as an individual user or as part of a group. When you add a user to an existing group, the groups’s policies apply to the user.

1. From the **Control Panel**, click **Users**.
The right pane lists the groups that are available in your system.
1. Enter the following details:
* Username – the string with which the user can log into the system. This is not editable in future.
* Email – The email ID to which the password reset link is sent. This can be updated, if required
* Password – The password for the username. This can be updated later. 
* Group – The list of available groups to which the user can be added. You can also add a new group from here.
* Group Admin For – The groups for which the user is the administrator.
* Quota – The maximum disk space permitted for the user.

3. Click **Create**.

You can customize the options on the page by clicking the gear icon at the bottom of the left pane and choosing the desired options.



 

