# Udacity Linux Server Configuration Project

This project, the setup and configuration of a Linux web server, was developed as part of the Udacity Full Stack Web Developer Nanodegree Program. The server was created as an Ubuntu Linux instance on Amazon Lightsail. Following the project guidelines, the server was secured and configured to host a web application complete with a database server.

**Note: The server has since been taken down. This repository will remain as a record of the steps taken to complete the project.**

## Connection Info

* IP Address: ~~`52.32.92.198`~~
* SSH Port: ~~`2200`~~
* App URL: ~~`http://ec2-52-32-92-198.us-west-2.compute.amazonaws.com`~~

## Configuration Summary

#### General Security

* All installed packages were updated to most recent versions. The following commands were entered at the server terminal:

  1.  `sudo apt-get update`
  2.  `sudo apt-get upgrade`
  3.  `sudo apt-get dist-upgrade`
  4.  `sudo apt-get autoremove`

* The SSH port was changed from 22 to 2200. This was accomplished by making the necessary change in `/etc/ssh/sshd_config`. After making any changes to this file, including this port change, the SSH service was restarted by running the command `sudo service ssh restart`.

* The Lightsail firewall was configured to allow the SSH port change.

* The server firewall (UFW) was enabled (following configuration) to only allow incoming requests for SSH (port 2200), HTTP (port 80), and NTP (port 123). To accomplish this, the following commands were entered at the server terminal (after confirming that the UFW was disabled by running `sudo ufw status`):

  1.  `sudo ufw default deny incoming`
  2.  `sudo ufw default allow outgoing`
  3.  `sudo ufw allow 2200/tcp`
  4.  `sudo ufw allow www`
  5.  `sudo ufw allow ntp`
  6.  `sudo ufw enable`

* Again, the Lightsail firewall was adjusted accordingly.

#### User Management

* Key-based SSH authentication is enforced since password login is disabled by default on the Lightsail Ubuntu instance. This was verified by checking the `/etc/ssh/sshd_config` file.

* Remote login of the root user was disabled. This was accomplished by making the appropriate change in `/etc/ssh/sshd_config`. Again, after making any changes to this file, the SSH service was restarted by running the command `sudo service ssh restart`.

* A new user account named `grader` was created using the command `sudo adduser grader`.

* In order to login, an SSH key pair was generated for the `grader` user. To complete the process, the following steps were taken:

  1.  Created a `.ssh` directory: `sudo mkdir /home/grader/.ssh`
  2.  Created an `authorized_keys` file to contain the user's public key obtained from the SSH key pair generation: `sudo touch /home/grader/.ssh/authorized_keys`
  3.  Copied and pasted the public key into the `authorized_keys` file.

* File permissions, owner, and group were changed on the `grader` user's `.ssh` directory and `authorized_keys` file. The following commands were run:

  1.  `sudo chmod 700 /home/grader/.ssh`
  2.  `sudo chmod 600 /home/grader/.ssh/authorized_keys`
  3.  `sudo chown grader /home/grader/.ssh`
  4.  `sudo chgrp grader /home/grader/.ssh`
  5.  `sudo chown grader /home/grader/.ssh/authorized_keys`
  6.  `sudo chgrp grader /home/grader/.ssh/authorized_keys`

* The `grader` user was given sudo access by completing the following steps:
  1.  Created a `grader` file in the `/etc/sudoers.d` directory: `sudo touch /etc/sudoers.d/grader`
  2.  Added the following contents to the `grader` file: `grader ALL=(ALL) NOPASSWD:ALL`

#### Web Server

* The local timezone was verified to be set to UTC (with NTP synchronization) by running the command `timedatectl`.

* The Apache HTTP Server was installed using the command `sudo apt-get install apache2`.

* The Apache application handler mod_wsgi was installed. Since the Item Catalog project was built with Python 3, the Python 3 mod_wsgi package was chosen. The package was installed by running the command `sudo apt-get install libapache2-mod-wsgi-py3`.

* The Apache server was configured to handle requests using the WSGI module. This was done by adding the line `WSGIScriptAlias / /var/www/html/catalog/app.wsgi` to the `/etc/apache2/sites-enabled/000-default.conf` file. Apache was then restarted using the command `sudo apache2ctl restart`.

* PostgreSQL was installed using the command `sudo apt-get install postgresql`.

* Remote connections to PostgreSQL are disabled by default. This was verified by checking the `/etc/postgresql/9.5/main/pg_hba.conf` file.

* A new database user named `catalog` was created with limited permissions. This was accomplished by running the command `sudo -u postgres createuser --interactive`.

* A new database, `catalog`, was created using the command `sudo -u postgres createdb catalog`.

* Using Git, which was installed by default on the Lightsail Ubuntu instance, the Item Catalog project was cloned. This was done by running the command `sudo git clone https://github.com/abequinonez/udacity-item-catalog.git /var/www/html/catalog`.

* pip3, the Python 3 package manager, was installed using the command `sudo apt-get install python3-pip`.

* In order to run the Item Catalog project, it was necessary to install a few Python packages using the following commands:

  1.  `sudo pip3 install flask`
  2.  `sudo pip3 install sqlalchemy`
  3.  `sudo pip3 install oauth2client`
  4.  `sudo pip3 install psycopg2`

* The Item Catalog project was configured to connect to the PostgreSQL database.

* Both the Apache server and the Item Catalog project were configured to work with each other.

## Software Installed

* Apache HTTP Server (apache2)
* Python 3 Apache WSGI Module (libapache2-mod-wsgi-py3)
* PostgreSQL (postgresql)
* Python 3 Package Manager (python3-pip)

## Python Packages Installed

* flask
* sqlalchemy
* oauth2client
* psycopg2

## Helpful Resources

* https://askubuntu.com/questions/449032/29-packages-can-be-updated-how
* https://www.godaddy.com/help/changing-the-ssh-port-for-your-linux-server-7306
* https://mediatemple.net/community/products/dv/204643810/how-do-i-disable-ssh-login-for-the-root-user
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04
* https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps
* https://www.digitalocean.com/community/tutorials/how-to-set-up-time-synchronization-on-ubuntu-16-04
* http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi
* https://jackhalpinblog.wordpress.com/2016/08/27/getting-your-python-3-flask-app-to-run-on-apache
* https://www.jakowicz.com/flask-apache-wsgi
