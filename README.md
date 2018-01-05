# Udacity Linux Server Configuration Project

This project, the setup and configuration of a Linux web server, was developed as part of the Udacity Full Stack Web Developer Nanodegree Program. The server was created as an Ubuntu Linux instance on Amazon Lightsail. Following the project guidelines, the server was secured and configured to host a web application complete with a database server.

## Configuration Summary

#### General Security
* All installed packages were updated to most recent versions. The following commands were entered at the server terminal:
    1. ```sudo apt-get update```
    2. ```sudo apt-get upgrade```
    3. ```sudo apt-get dist-upgrade```
    4. ```sudo apt-get autoremove```

* The SSH port was changed from 22 to 2200. This was accomplished by making the necessary change in ```/etc/ssh/sshd_config```. After making any changes to this file, including this port change, the SSH service was restarted by running the command ```sudo service ssh restart```.

* The Lightsail firewall was configured to allow the SSH port change.

* The server firewall (UFW) was enabled (following configuration) to only allow incoming requests for SSH (port 2200), HTTP (port 80), and NTP (port 123). To accomplish this, the following commands were entered at the server terminal (after confirming that the UFW was disabled by running ```sudo ufw status```):
    1. ```sudo ufw default deny incoming```
    2. ```sudo ufw default allow outgoing```
    3. ```sudo ufw allow 2200/tcp```
    4. ```sudo ufw allow www```
    5. ```sudo ufw allow ntp```
    6. ```sudo ufw enable```

* Again, the Lightsail firewall was adjusted accordingly.