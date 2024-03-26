# Ubuntu

Good to create a user or group for each service

## Apache

Config user/group to grant permission.

etc/apache2
etc/apache2/sites-available

a2ensite/a2dissite # enable/disable virtual host config

## SQL

mysql -u username -p password #

CREATE DATABASE database;
USE database;

CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL ON database.* TO 'user'@'localhost';

### Pitfall

Setup the firewall before remote connection.

## Command

ufw # firewall

## ssh

~/.ssh/authorized_keys # public key for login

## User

passwd # change password
gpasswd # change group
